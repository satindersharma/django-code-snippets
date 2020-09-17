# django-code-snippets






#### Django RuntimeWarning
 Django raises a warning when you attempt to save a naive datetime to the database:
```
 RuntimeWarning: DateTimeField ModelName.field_name received a naive
datetime (2012-01-01 00:00:00) while time zone support is active.
```

to handel this warning

use

```python
import django
from sys import argv
import os
from django.utils import timezone
import random
# from time import sleep
from faker import Faker
import warnings
# from django.contrib.auth.hashers import make_password

# the below line is copied from wsgi file
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'CelecUserProject.settings')
django.setup()


def random_data(ne=100, es=3):
    for _ in range(ne):
        f = Faker('en_US')
        data = {
            # "date_time": f.date_time_this_century(before_now=True, after_now=False, tzinfo=None),
            # "date_time": timezone.now(), 
            "date_time": f.past_datetime(start_date='-0d', tzinfo=None), # '-30d' for last 30 day data '-0d' means today '-1d' means from yesterday to today
            # "date_time": f.date_time_this_year(before_now=True, after_now=False, tzinfo=None),
            "saving": random.randrange(0, 101),
            "usage": random.randrange(0, 101),
            "energy": round(random.uniform(0, 101), 2),
            "power_factor": round(random.uniform(0, 101), 3),
            "thd": round(random.uniform(0, 101), 2),
            "tdi": round(random.uniform(0, 101), 2),
        }
        with warnings.catch_warnings():
            warnings.simplefilter("ignore")
            DashboardTable.objects.create(**data)
            # sleep(es)
    print(f'{ne} new data created successfully')


if __name__ == '__main__':
    from ermapp.models import DashboardTable
    print('filling some random data')
    if len(argv) == 1:
        random_data()
    elif len(argv) <= 3:
        # print(argv)
        # when you do python new_data.py 23 here argv[0] is random_post.py, argv[1] is 23
        if len(argv) == 2:
            random_data(ne=int(argv[1]))
        else:
            random_data(ne=int(argv[1]), es=int(argv[2]))
    else:
        random_data()
```
