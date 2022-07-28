.PHONY: all download clean

all: download

download: reports.meta4
	aria2c --save-cookies=cookies.txt --dry-run 'https://documents-dds-ny.un.org/prod/ods_mother.nsf?Login=&Username=freeods2&Password=1234'
	aria2c --load-cookies=cookies.txt --allow-overwrite=true --check-integrity=true $<
	rm cookies.txt

clean:
	find . -type f \( -name \*.pdf -or -name \*.doc \) -delete
