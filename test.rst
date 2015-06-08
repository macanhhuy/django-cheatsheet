=======
Testing
=======

.. code-block:: python

    self.assertEqual(a, b, msg=None)


    rsp = self.client.get(url, [follow=True])
    rsp = self.client.post(url, data, [follow=True])

    rsp.content is a byte string
    rsp['HeaderName']
    rsp.context['template_var']

    assert self.client.login(**login_parms)
    login(email='foo@example.com', password='cleartextpassword')



# http://docs.python.org/library/unittest.html
# https://docs.djangoproject.com/en/dev/topics/testing/

.. code-block:: python

from django.test import TestCase
from django.contrib.auth.models import User


class XxxTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user('tester', 'test@example.com', 'testpass')

    def test_something(self):
        response = self.client.get(show_timemap)
        self.assertEqual(response.status_code, 200)
        self.assertEqual(response.context['lat'], '123.123')
        self.assertEqual(response.context['lon'], '456.456')
        self.assertContains(response, "some text")
        self.assertNotContains(response, "other text")
        self.assertIsNone(val)
        self.assertIsNotNone(val)
        self.assertIn(thing, iterable)  # Python >= 2.7
        self.assertNotIn(thing, iterable)  # Python >= 2.7


# Test uploading a file
https://docs.djangoproject.com/en/1.7/topics/testing/tools/#django.test.Client.post

Submitting files is a special case. To POST a file, you need only provide the file field name as a key, and a file handle to the file you wish to upload as a value. For example:

>>> c = Client()
>>> with open('wishlist.doc') as fp:
...     c.post('/customers/wishes/', {'name': 'fred', 'attachment': fp})

(The name attachment here is not relevant; use whatever name your file-processing code expects.)

Note that if you wish to use the same file handle for multiple post() calls then you will need to manually reset the file pointer between posts. The easiest way to do this is to manually close the file after it has been provided to post(), as demonstrated above.

You should also ensure that the file is opened in a way that allows the data to be read. If your file contains binary data such as an image, this means you will need to open the file in rb (read binary) mode.

