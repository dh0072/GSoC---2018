In this summer, I work on two projects:
1. RTEMS Release Notes Generator (ticket: #3314)
2. RTEMS POSIX User Guide Generator (ticket: #3333)

It is a fruitful summer, let’s take a look at what I have done! 😊

First project: RTEMS POSIX User Guide Generator (ticket: #3333)

Ticket’s link: https://devel.rtems.org/ticket/3314

My Github repository: https://github.com/dh0072/ReleaseNotesGenerator

Goal: This project aims to automatically create the RTEMS release notes for a release from the Trac data by using XML parser (Python)

Completed tasks:
1. Provided a command “python2 release_notes.py --milestone_id XXX” to directly generate a release notes in Markdown format from Trac
2. Generated Markdown version of release notes “tickets.md”
3. Created a python class “unicode_dict_reader.py” to make the program be compatible with both Python 2.7 and Python 3.6 
4. Created a Python class “tickets.py” to get all needed data from a ticket’s page and a milestone’s page
5. Created a Python class “markdown.py” to limit max column width to 20 characters and max number of columns to 4. Therefore, release notes can fit into A4 size.
6. Fetched needed information by parsing XML page
7. Calculated the tickets’ statistics for the given milestone

Second project: RTEMS POSIX User Guide Generator (ticket: #3333)

Ticket’s link: https://devel.rtems.org/ticket/3333

My Github repository: https://github.com/dh0072/NewlibMarkup2SphinxConverter

Patch’s link: https://github.com/dh0072/GSoC2018GithubIO/blob/master/makedoc2rst_patch.md

Goal: This project aims to automatically convert Newlib markup to Sphinx output and integrate with POSIX users guide

Completed tasks:
1. Created Command Line Interface (Python argument parser) to specify path of C source file and path of destination rst markup file
2. Provided rst utilities to register commands (FUNCTION, INDEX, SYNOPSYS, etc) and their corresponding processors methods
3. Created class (makedoc2rst) to parse C-style block comments with makedoc format in C source file with Regular Expression matching
4. Extracted commands and its corresponding text in C source file
5. Generated rst markup based on command type and save it to destination rst file

Though it is almost the end of GSoC project, I will keep an eye out for any feature requests bug fixes related to them. I believe it is one of the sprits of open source project – keep making contribution to the project and always maintain the code!

I am grateful to meet my mentors! Thanks to their professionalism, patience and passion, so I can finish these projects under their supervisions and have such a fruitful summer! 😊
