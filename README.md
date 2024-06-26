# Cookbook

FFUF is one of the latest and by far the fastest fuzzing open source tool out there.But before we begin, let’s first try to understand what fuzzing really is.

Fuzzing is the automatic process of giving random input to an application to look for any errors or any unexpected behavior. But finding hidden directories and files on a web server can also be categorized under fuzzing.

The tool is versatile and can be used for a variety of purposes. Some of its use cases are:

    General Directory discovery with option to fuzz at any place in the URL.
    VHOST discovery without DNS Records
    Fuzzing using various HTTP methods.

We’ll talk about the installation and then move on to the usage of the tool.

Installation

    The tool can be easily installed by downloading the compatible binary in the form of a tar.gz file from the Releases page of ffuf on Github.
    It can also be installed by using the go get command.

    go get github.com/ffuf/ffuf

Golang compiler is needed to use this method which can be easily installed using apt-get in Linux.

    apt-get install golang

Don’t forget to add the go/bin directory in PATH variable to use the tool from any directory.

Usage

Ffuf offers many options for fuzzing.

The position to be fuzzed should be indicated by the FUZZ word in the ffuf command.

Directory and File Discovery

The directories on the website can be discovered with the following command which gives ffuf a word-list with -w flag and URL with -u command which contains the word FUZZ, that signifies the position to be fuzzed.

    ffuf -w wordlist.txt -u http://website.com/FUZZ

For file discovery, the same command can be used and for using specific extensions with the word-list’s entries, the -e flag can be used.

    ffuf -w wordlist.txt -u http://website.com/FUZZ -e .aspx,.html,.php,.txt

Ffuf also gives option to get output only of responses with specific status code, amount of lines, response size, amount of words as well as the response which matches a regex pattern.

A few examples of flags for the same are:

    -mc : to specify Status code.
    -ml: to specify amount of lines in response
    -mr: to specify regex pattern
    -ms: to specify response size
    -mw: to specify amount of words in response

Here are a few demonstrations to make it clearer and easier to understand.

For getting output of responses with status code 200 and 302 only, use:

    ffuf -w wordlist.txt -w http://website.com/FUZZ -e .aspx,.html -mc 200,302

The URL parameters which end with FUZZ are also supported with the feature of recursion which when activated using -recursion flag would try to fuzz the given URL and then fuzz furthermore inside the directories it found in the primary fuzz.

The depth of recursion can also be specified by -recursion-depth flag.

The -maxtime flag offers to end the ongoing fuzzing after the specified time in seconds.

    ffuf -w wordlist.txt -u http://website.com/FUZZ -maxtime 60

The above command will work for 60 seconds and then kill itself even if the word-list is not finished.

-maxtime-job is used with -recursion flag and is used to specify the time (in seconds) for each new job that would be created for each directory found.

The number of default threads on which ffuf works are 40 and can be changed with the -t flag in the command.

VHOST Discovery

This tool is able to find subdomains without DNS records at blazing fast speeds.

The tool utilizes the Host header in an HTTP request to look for subdomains. The -H flag is used to specify HTTP request headers. Please note that multiple -H flags are allowed.

    ffuf -w subdomains.txt -u http://website.com/ -H “Host: FUZZ.website.com”

If the tool gives many subdomains as output and most of them are not present in reality, then the filter options offered by the tool can be used.

Note either the most common size, words or lines for the false positive responses and then specify them in a filter. Use:

-fw : to filter by the amount of words

-fl : to filter by the number of lines

-fs : to filter by the size of the response

-fc : to filter by the status code

-fr : to filter by the regex pattern

As in the above image there are many false positives and most of them have size:12454, words:3913, lines:421.

So we can filter these responses with the filter flags.

    ffuf -w sublists.txt -u http://website.com/ -H “Host: FUZZ.website.com” -fw 3913

