During my participation of google summer of code this year, the first project I work on is RTEMS release notes generator. Why do we need a release note tools in the RTEMS project?

1.	Missing data in release notes. Currently, the release notes could be regularly used, but some essential data are missed in the release notes. Therefore, we need to extract all of the needed data from a ticket’s page to put on the release notes.

2.	Formatting issues. Some data is not readable because of formatting issues. Therefore, it is also one of my goals to provide a better formatting of the ticket. Also, date formatting is not that reasonable. For example, the date is a local setting but not a full date.

The release notes tool fits in the RTEMS project quite well, because release notes can be generated automatically from the Trac data including all of the needed data now. Therefore, the release notes is not only more readable, but also contains more essential data.
