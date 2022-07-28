.PHONY: all download clean update_metalink

SIZE ?= stat -f '%z'
MD5 ?= md5 -q
SHA512 ?= sha2 -q -512

all: download

download: reports.meta4
	aria2c --save-cookies=cookies.txt --dry-run 'https://documents-dds-ny.un.org/prod/ods_mother.nsf?Login=&Username=freeods2&Password=1234'
	aria2c --load-cookies=cookies.txt --allow-overwrite=true --check-integrity=true $<
	rm cookies.txt

clean:
	find . -type f \( -name \*.pdf -or -name \*.doc \) -delete

update_metalink:
	xml sel -N 'm=urn:ietf:params:xml:ns:metalink' -t -v '/m:metalink/m:file/@name' -nl reports.meta4 | while read file; do \
		xml ed -S -L -N 'm=urn:ietf:params:xml:ns:metalink' \
			-d '/m:metalink/m:file[@name="'"$${file}"'"]/m:size' \
			-s '/m:metalink/m:file[@name="'"$${file}"'"]' -t elem -n size -v "$$($(SIZE) "$${file}")" \
			-d '/m:metalink/m:file[@name="'"$${file}"'"]/m:hash[@type="md5"]' \
			-s '/m:metalink/m:file[@name="'"$${file}"'"]' -t elem -n hash -v "$$($(MD5) "$${file}")" \
			-i '(/m:metalink/m:file[@name="'"$${file}"'"]/hash)[last()]' -t attr -n type -v md5 \
			-d '/m:metalink/m:file[@name="'"$${file}"'"]/m:hash[@type="sha-512"]' \
			-s '/m:metalink/m:file[@name="'"$${file}"'"]' -t elem -n hash -v "$$($(SHA512) "$${file}")" \
			-i '(/m:metalink/m:file[@name="'"$${file}"'"]/hash)[last()]' -t attr -n type -v sha-512 \
			reports.meta4; \
	done
	xml format --indent-spaces 4 reports.meta4 > reports.meta4.tmp
	mv reports.meta4.tmp reports.meta4
