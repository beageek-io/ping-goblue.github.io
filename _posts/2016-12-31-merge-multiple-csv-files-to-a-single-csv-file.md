---
layout: post
title: Merge multiple csv files to a single csv file
category: bash
---

Recently, I tried to merge multiple csv files (with the same header) into a single csv file without header.

For example, I have two csv files, `hi.csv` and `hi1.csv`:

```bash
$ cat hi.csv
uuid,email
"i-am-uuid","i-am-email"

$ cat hi1.csv
uuid,email
"i-am-uuid-in-hi1.csv","i-am-email-in-hi1.csv"
```

And I want to merge them to be:

```bash
"i-am-uuid","i-am-email"
"i-am-uuid-in-hi1.csv","i-am-email-in-hi1.csv"
```

You can achieve this by:

```bash
$ find . -name "hi*.csv" | xargs -n 1 tail -n +2
"i-am-uuid","i-am-email"
"i-am-uuid-in-hi1.csv","i-am-email-in-hi1.csv"
```

And then you can pip the result into a new file:

```bash
$ find . -name "hi*.csv" | xargs -n 1 tail -n +2 > new_file.csv
$ cat new_file.csv
"i-am-uuid","i-am-email"
"i-am-uuid-in-hi1.csv","i-am-email-in-hi1.csv"
```

For more information about `xargs`, please see [here](http://www.thegeekstuff.com/2013/12/xargs-examples)

**What if I want the new file to have a header?**

Well, after you have the `new_file.csv`, you can insert the header into the beginning of the `new_file.csv`:

```bash
$ echo -e "$(head -n 1 hi1.csv)\n$(cat new_file.csv)" > new_file.csv
$ cat new_file.csv
uuid,email
"i-am-uuid","i-am-email"
"i-am-uuid-in-hi1.csv","i-am-email-in-hi1.csv"
```

You can also combine these two steps into one single command:

```bash
$ echo -e "$(head -n 1 hi1.csv)\n$(find . -name 'hi*.csv' | xargs -n 1 tail -n +2 )" > new_file.csv
$ cat new_file.csv
uuid,email
"i-am-uuid","i-am-email"
"i-am-uuid-in-hi1.csv","i-am-email-in-hi1.csv"
```

I hope it is useful for you.
