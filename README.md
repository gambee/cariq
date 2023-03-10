# CarIQ Golang Data Code Challenge

A crucial part of the CarIQ platform requires gathering, formatting and storing data generated by external data providers for use in various platform operations. The data providers generate data updates for connected devices as they become available on the device, and each data update may only include data for some available fields.

Each update message includes a device identifier and a set of data fields, and are formatted as JSON data. Each device is uniquely identified by the string identifier in the `device` data field, and may include speed, heading and position data fields and a generated timestamp. Note that messages could be passed in an order different than they are generated, and that the type of the `position` data fields may vary between strings and numbers because of differences in the data provider implementations.

Build a service invoked by command line called `process` in Golang (1.18+) to accept one data message at a time, process and store the data in relational database tables and return the latest known state of the device updated. The data message should be accepted as a string passed on the command line to the service, and only needs to handle a single data update per invocation. The service should return the latest known state (the latest values of all non-null data fields) for the device as a JSON message printed to stdout.

For example:

```
$ process '{"device": "A123", "generated": "2022-01-01 15:00:00.000", "speed": 48.7, "heading": 101}'

{"device": "A123", "speed": 48.7, "heading": 101}

$ process '{"device": "A123", "generated": "2022-01-01 15:01:00.000", "position" : {"lat": -80.0101, "long": 40.0101}}'

{"device": "A123", "speed": 48.7, "heading": 101, "position" : {"lat": -80.0101, "long": 40.0101}}

$ process '{"device": "A123", "generated": "2022-01-01 15:02:00.000", speed: 0}'

{"device": "A123", "speed": 0, "heading": 101, "position" : {"lat": -80.0101, "long": 40.0101}}

$ process '{"device": "B345", "generated": "2022-01-01 15:02:00.000", "speed": 21.55, "position" : {"lat": "-78.0101", "long": "42.0101"}}'

{"device": "B345", "speed": 21.55, "position" : {"lat": -78.0101, "long": 42.0101}}

$ process '{"device": "B345", "generated": "2022-01-01 15:01:00.000", "speed": 35.11, "heading": 45}'

{"device": "B345", "speed": 21.55, "heading": 45, "position" : {"lat": -78.0101, "long": 42.0101}}
```

Utilize whatever language features you like to ensure your code is well structured and readable, but "production-ready" code is not expected for this exercise (no concern needs to be given to code deployment or module structures). The code should be runnable and functional but will only be evaluated or run manually in a isolated local environment. The service or code should contain anything required to create the data schema it uses for storage, assume the database instance is Postgres (version 13+).

If you choose, you may also add any additional functionality to the service (for example to allow the retrieval of data or aggregates / statistics). However, the exercise is intended to take roughly 2-3 hours of work, so prioritize ensuring the core functionality is well implemented and the code showcases your development abilities rather than adding extra features. Please do not share or publish this challenge or your solution to it with anyone outside CarIQ.
