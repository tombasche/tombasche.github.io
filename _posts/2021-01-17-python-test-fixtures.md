---
layout: archive
title:  "Test Fixtures in Python"
---

Lately as part of my job I'd been experimenting with better ways of managing test data in Python. The knee-jerk reaction I think of a lot of developers (myself included) is to just duplicate code because "test code isn't real code". Which is true in many respects, but it doesn't mean readability should suffer. 

There's some clever things `pytest` lets you do specifically which I'll touch on below.

## Sharing is caring

In `pytest` you can use a `conftest.py` file to define fixtures that are shared across modules, and even entire test directories. This file is documented [here](https://docs.pytest.org/en/stable/fixture.html#conftest-py-sharing-fixture-functions). No further work is required; they are automagically discovered by `pytest`! 

A great example of this might be some sort of database client used in tests which require a database connection. This can be defined at the highest level of the folder structure, and will be discoverable by all tests beneath it. 

```sh
db-tests/
├── test_queries/
│   └── test_something.py 
├── test_inserts/
│   └── test_something_else.py
└── conftest.py # this has your db fixture in it
```

You simply define a fixture like so:

```python
@pytest.fixture(scope="session")  # means it will only be destroyed once
def db_client():
    c = DbClient()
    c.connect()
    yield c
    c.close()
```

In this case I have abstracted away the details, but the pattern of setup, yield the object, clean up leads nicely into the next point.
## Self-cleaning fixtures

In those instances where you have setup code that has to clean up after itself I've found a great pattern which doesn't involve a heap of duplication, and reads quite nicely.

```python
@pytest.fixture
def sample_data() -> Iterable[Any]:
    data = insert_sample_data()
    yield data
    delete_sample_data()
```

What this means for your tests is this: you call this fixture, which will call `insert_sample_data`, return the data, and then once it is finished with the fixture it will call `delete_sample_data`.

Here's a contrived example:

```python
def test_filtering_removes_duplicates(sample_data):
    filtered = filter_duplicates(sample_data)
    assert has_duplicates(filtered) is False
```

Once this test completes (it fails or passes) the `delete_sample_data` function will be called in the test fixture, meaning no awkward manual steps required to clean up test data.

## Too many options

I would be interested to know what the experience has been generally with testing frameworks, especially for production-grade applications which require a decent level of unit and integration testing. Pytest seems to be the favourite these days, but perhaps there's something better out there!