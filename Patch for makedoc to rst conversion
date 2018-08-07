```
---
 .gitignore              |   3 ++
 README.md               |   2 +
 gen_rst_from_makedoc.py |  61 ++++++++++++++++++++++++++++
 makedoc2rst.py          |  99 +++++++++++++++++++++++++++++++++++++++++++++
 rst.py                  | 104 ++++++++++++++++++++++++++++++++++++++++++++++++
 5 files changed, 269 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 README.md
 create mode 100755 gen_rst_from_makedoc.py
 create mode 100644 makedoc2rst.py
 create mode 100644 rst.py

diff --git a/.gitignore b/.gitignore
new file mode 100644
index 0000000..f707fbd
--- /dev/null
+++ b/.gitignore
@@ -0,0 +1,3 @@
+.idea/*
+__pycache__/*
+*.rst
diff --git a/README.md b/README.md
new file mode 100644
index 0000000..64a3ba9
--- /dev/null
+++ b/README.md
@@ -0,0 +1,2 @@
+# NewlibMarkup2SphinxConverter
+This repo contains code for NewlibMarkup2SphinxConverter
diff --git a/gen_rst_from_makedoc.py b/gen_rst_from_makedoc.py
new file mode 100755
index 0000000..8e4d9b0
--- /dev/null
+++ b/gen_rst_from_makedoc.py
@@ -0,0 +1,61 @@
+#!/usr/bin/env python
+#
+# RTEMS Tools Project (http://www.rtems.org/)
+# Copyright 2018 Danxue Huang (danxue.huang@gmail.com)
+# All rights reserved.
+#
+# This file is part of the RTEMS Tools package in 'rtems-tools'.
+#
+# Redistribution and use in source and binary forms, with or without
+# modification, are permitted provided that the following conditions are met:
+#
+# 1. Redistributions of source code must retain the above copyright notice,
+# this list of conditions and the following disclaimer.
+#
+# 2. Redistributions in binary form must reproduce the above copyright notice,
+# this list of conditions and the following disclaimer in the documentation
+# and/or other materials provided with the distribution.
+#
+# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
+# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
+# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
+# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
+# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
+# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
+# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
+# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
+# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
+# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
+# POSSIBILITY OF SUCH DAMAGE.
+#
+
+import argparse
+import makedoc2rst
+
+
+def get_parser():
+    parser = argparse.ArgumentParser(
+        description='Convert newlib style markup to rst markup'
+    )
+    parser.add_argument(
+        '-c',
+        '--c_file_path',
+        type=str,
+        help='Path of c source file with newlib style comments',
+    )
+    parser.add_argument(
+        '-r',
+        '--rst_file_path',
+        type=str,
+        help='Path of destination file with rst markup',
+    )
+    return parser
+
+
+def main(c_file, rst_file):
+    makedoc2rst.makedoc2rst(c_file, rst_file).convert()
+
+
+if __name__ == '__main__':
+    args = get_parser().parse_args()
+    main(args.c_file_path, args.rst_file_path)
diff --git a/makedoc2rst.py b/makedoc2rst.py
new file mode 100644
index 0000000..d887323
--- /dev/null
+++ b/makedoc2rst.py
@@ -0,0 +1,99 @@
+#!/usr/bin/env python
+#
+# RTEMS Tools Project (http://www.rtems.org/)
+# Copyright 2018 Danxue Huang (danxue.huang@gmail.com)
+# All rights reserved.
+#
+# This file is part of the RTEMS Tools package in 'rtems-tools'.
+#
+# Redistribution and use in source and binary forms, with or without
+# modification, are permitted provided that the following conditions are met:
+#
+# 1. Redistributions of source code must retain the above copyright notice,
+# this list of conditions and the following disclaimer.
+#
+# 2. Redistributions in binary form must reproduce the above copyright notice,
+# this list of conditions and the following disclaimer in the documentation
+# and/or other materials provided with the distribution.
+#
+# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
+# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
+# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
+# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
+# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
+# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
+# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
+# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
+# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
+# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
+# POSSIBILITY OF SUCH DAMAGE.
+#
+
+import re
+import rst
+
+
+class makedoc2rst():
+    """ Convert c source file with makedoc markup to rst markup file
+    c_file: c source file containing block comments (/* */) at the beginning
+    rst_file: destination file with rst markup
+    """
+    def __init__(self, c_file, rst_file):
+        self.c_file = c_file
+        self.rst_file = rst_file
+
+    def convert(self):
+        """ Implementation of converting c file to rst file """
+        rst_str = ''
+        with open(self.c_file, 'r') as c_file:
+            # Get comments inside of /* */
+            comments = self._extract_comments(c_file.read())
+            # Parse comments
+            command_text_dict = self._extract_command_and_text(comments)
+            # Process text based on command type
+            for command, text in command_text_dict.items():
+                rst_str += rst.get_command_processor(command)(command, text)
+
+        with open(self.rst_file, 'w') as rst_file:
+            rst_file.write(rst_str)
+        return rst_str
+
+    def _is_command(self, s):
+        """
+        A command is a single word of at least 3 characters, all uppercase
+        :param s: input string
+        :return: True if s is a single command, otherwise False
+        """
+        return True if re.match('^[A-Z_]{3,}\s*$', s) else False
+
+    def _extract_comments(self, content):
+        """
+        Extract content inside of /* */
+        :param content: input file content
+        :return: extracted comments
+        """
+        comments = ''
+        comments_match = re.match('/\*(\*(?!/)|[^*])*\*/', content)
+        if comments_match:
+            wrapped_comments = comments_match.group()
+            comments = wrapped_comments.lstrip('/*').rstrip('*/').lstrip().rstrip()
+        return comments
+
+    def _extract_command_and_text(self, comments):
+        """
+        Extract command and text from input string content
+        :param comments: input string containing command and text
+        :return: a tuple containing command and text
+        """
+        command = ''
+        text = ''
+        command_text_dict = {}
+        for line in comments.splitlines():
+            if self._is_command(line):
+                if command and text:
+                    command_text_dict[command] = text
+                command = line.rstrip()
+                text = ''
+            else:
+                text += line + '\n'
+        return command_text_dict
diff --git a/rst.py b/rst.py
new file mode 100644
index 0000000..2531f46
--- /dev/null
+++ b/rst.py
@@ -0,0 +1,104 @@
+#
+# RTEMS Tools Project (http://www.rtems.org/)
+# Copyright 2018 Danxue Huang (danxue.huang@gmail.com)
+# All rights reserved.
+#
+# This file is part of the RTEMS Tools package in 'rtems-tools'.
+#
+# Redistribution and use in source and binary forms, with or without
+# modification, are permitted provided that the following conditions are met:
+#
+# 1. Redistributions of source code must retain the above copyright notice,
+# this list of conditions and the following disclaimer.
+#
+# 2. Redistributions in binary form must reproduce the above copyright notice,
+# this list of conditions and the following disclaimer in the documentation
+# and/or other materials provided with the distribution.
+#
+# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
+# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
+# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
+# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
+# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
+# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
+# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
+# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
+# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
+# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
+# POSSIBILITY OF SUCH DAMAGE.
+#
+
+
+import re
+
+
+def register_processor_command():
+    return {
+        'FUNCTION': gen_function_summary,
+        'SYNOPSIS': gen_code_block,
+        'ANSI_SYNOPSIS': gen_code_block,
+        'TRAD_SYNOPSIS': gen_code_block,
+        'TYPEDEF': gen_code_block,
+        'DESCRIPTION': gen_custom_directives,
+        'INDEX': gen_custom_directives,
+        'RETURNS': gen_custom_directives,
+        'PORTABILITY': gen_custom_directives,
+        'NOTES': gen_custom_directives,
+        'ERRORS': gen_custom_directives,
+        'BUGS': gen_custom_directives,
+        'WARNINGS': gen_custom_directives,
+        'QUICKREF': gen_nothing,
+        'MATHREF': gen_nothing,
+        'NEWPAGE': gen_nothing,
+        'START': gen_nothing,
+        'END': gen_nothing,
+        'SEEALSO': gen_custom_directives,
+    }
+
+
+def get_command_processor(command):
+    command_processor_dict = register_processor_command()
+    if command in command_processor_dict:
+        return command_processor_dict[command]
+    else:
+        print('Command {c} is not recognized, skip it'.format(c=command))
+        return gen_nothing
+
+
+def gen_function_summary(command, text):
+    function_name = extract_function_name(text)
+    summary = extract_summary(text)
+
+    title = '.. {f}:\n\n{f} - {s}\n'.format(
+        f=function_name,
+        s=summary.capitalize()
+    )
+    dashes = '-' * len(text) + '\n'
+    func_name_index = '.. index:: {name}\n'.format(name=function_name)
+    summary_index = '.. index:: {summary}\n\n'.format(summary=summary)
+    return title + dashes + func_name_index + summary_index
+
+
+def extract_function_name(text):
+    function_name = ''
+    function_name_match = re.match('\s+(<<(>(?!>)|[^>])*>>)', text)
+    if function_name_match:
+        function_name = function_name_match.group(1).lstrip('<<').rstrip('>>')
+    return function_name
+
+
+def extract_summary(text):
+    return text.split('---')[-1].rstrip()
+
+
+def gen_code_block(command, text):
+    return '**{c}:**\n\n.. code-block:: c\n\n{t}\n\n'.format(c = command, t=text)
+
+
+def gen_nothing(command, text):
+    return '\n\n'
+
+
+def gen_custom_directives(command, text):
+    return '**{c}:**\n\n{t}\n\n'.format(c=command, t=text)
+
```
