---
title: Tests with mocking of 3rd party API using sentry python library responses
---

If you are looking for python library for testing your consumption of 3rd party API you can give a try to this library (`responses` see [https://github.com/getsentry/responses](https://github.com/getsentry/responses)) allowing you to test arbitrary requests scenarios with mocked responses.

Idea is simply:

- you hardcode the desired response in some dict
- you create mock of the certain url so the real request does not go somewhere in the web, but uses the mocked data and provided status code etc. what you configured using `responses.add` and it is valid in the scope of decorated method with `@responses.activate`


Basic example:
-----------
Example test - which is testing either my method `reverse_geocoding(latitude, longitude)` extract desired things from the whole response it gets from Google Maps API:

{% highlight python %}
import json
import responses

from django.conf import settings
from django.test import TestCase

from utils.geocoding import reverse_geocoding, REVERSE_GEOCODING_URL


RESPONSE_GEOCODING_MOCK = {
    'results': [
        {
            'address_components': [
                {'long_name': '74', 'short_name': '74', 'types': ['street_number']},
                {'long_name': 'Mickiewicza', 'short_name': 'Mickiewicza', 'types': ['route']},
                {'long_name': 'Żoliborz', 'short_name': 'Żoliborz', 'types': ['political', 'sublocality', 'sublocality_level_1']},
                {'long_name': 'Warszawa', 'short_name': 'Warszawa', 'types': ['locality', 'political']},
                {'long_name': 'Warszawa', 'short_name': 'Warszawa', 'types': ['administrative_area_level_2', 'political']},
                {'long_name': 'mazowieckie', 'short_name': 'mazowieckie', 'types': ['administrative_area_level_1', 'political']},
                {'long_name': 'Poland', 'short_name': 'PL', 'types': ['country', 'political']}
            ],
            'formatted_address': 'Mickiewicza 74, Warszawa, Poland',
            'geometry': {'location': {'lat': 52.2790223, 'lng': 20.980648},
                         'location_type': 'ROOFTOP',
                         'viewport': {'northeast': {'lat': 52.2803712802915, 'lng': 20.9819969802915},
                                      'southwest': {'lat': 52.2776733197085, 'lng': 20.9792990197085}}
                         },
            'place_id': 'ChIJxXoMrOfLHkcRBAFsrjKGcMc',
            'types': ['street_address']
        }
    ],
    'status': 'OK'
}


class TestGoogleAPIGeocodingConsumer(TestCase):

    @responses.activate
    def test_geocoding(self):
        latitude, longitude = 52.27904, 20.980366
        responses.add(
            responses.GET,
            REVERSE_GEOCODING_URL.format(
                latitude=latitude,
                longitude=longitude,
                api_key=settings.GOOGLE_API_KEY
            ),
            body=json.dumps(RESPONSE_GEOCODING_MOCK),
            content_type="application/json")

        response = reverse_geocoding(latitude, longitude)
        self.assertEqual(response, {'address_city': 'Warszawa',
                                    'address_country': 'Poland',
                                    'address_number': '74',
                                    'address_street': 'Mickiewicza'})
{% endhighlight %}

the tested method for reference `utils/geocoding.py`:

{% highlight python %}
REVERSE_GEOCODING_URL = "https://maps.googleapis.com/maps/api/geocode/json?latlng={latitude},{longitude}&key={api_key}"


def reverse_geocoding(latitude, longitude):

    url = REVERSE_GEOCODING_URL.format(
        latitude=latitude,
        longitude=longitude,
        api_key=settings.GOOGLE_API_KEY
    )
    response = requests.get(url)
    info = response.json()
    address = {}

    for component in info['results'][0]['address_components']:
        if 'street_number' in component.get('types'):
            address['address_number'] = component['long_name']
        if 'route' in component.get('types'):
            address['address_street'] = component['long_name']
        if 'locality' in component.get('types'):
            address['address_city'] = component['long_name']
        if 'country' in component.get('types'):
            address['address_country'] = component['long_name']

    return address
{% endhighlight %}

More sophisticated one:
-------------------
Let's use custom calback doing something based on requests, and in this example setting value in session.

{% highlight python %}
import json
import responses
import requests

from django.test import TestCase


PAYMENT_ID_PAY_PAL = 'PAY_PAL'
PAYMENT_METHOD_KEY_NAME = 'SESSION_KEY_NAME_FOR_PAYMENT_METHOD'


class MockedPaymentMethodTest(TestCase):

    @responses.activate
    def test_setting_payment_method_id_in_session(self):
        # make sure that after PUT request with payment method
        # we store info about it in session
        url = 'http://example.com:8080/payment_metod/'
        payment_method_id = PAYMENT_ID_PAY_PAL

        s = requests.Session()

        def request_callback(request):
            data = json.loads(request.body)
            resp_body = {u'status': u'Ok'}

            s.cookies[PAYMENT_METHOD_KEY_NAME] = data.get('payment_method_id')
            return 201, {}, json.dumps(resp_body)

        responses.add_callback(
            responses.PUT,
            url,
            callback=request_callback,
            content_type='application/json',
        )
        response = s.put(
            url,
            json.dumps({'payment_method_id': payment_method_id}),
            headers={'content-type': 'application/json'}
        )
        self.assertEqual(response.status_code, 201)
        self.assertEqual(
            s.cookies[PAYMENT_METHOD_KEY_NAME], payment_method_id
        )
{% endhighlight %}