Design development of RTEMS release generator:

1.	At first, we need to figure out what our goal is. Our goal can be divided to a couple of tasks. For example, we need to include all needed data in the release notes, to fix formatting issues, etc.

2.	After our goal is divided into a couple of tasks, we need to figure out which one is the most essential one. At this point of time, getting needed data is the most important thing because some of needed data is missing in the release notes.

3.	Thirdly, we need to figure out what is the best solution for our problem. In general, there are two ways to get needed data. One is to parse HTML page, the other way is to parse the XML in RSS feed. Finally, we decide to use XML parser to parse the XML. For more detailed reasons, please read my other post in this blog: parse HTML page VS parse XML page.
