.PHONY: all

all: string_funcs.html math_funcs.html bloom_filter1.html \
	blob_funcs.html pcre2_funcs.html posix_re_funcs.html json_funcs.html

%.html:  %.md
	pandoc -f markdown -t html5 --mathjax -s --toc --toc-depth=2 -o $@ $<
