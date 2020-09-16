FROM adoptopenjdk/openjdk8:alpine

ENV pmki_version=1.0.0
ENV st_version=8.0

COPY pmki-${pmki_version}-full.zip /tmp
RUN set -eux ; \
	mkdir /opt/pmki ; \
	mkdir /opt/pmki/data ; \
	cd /opt/pmki ; \
	unzip -q /tmp/pmki-${pmki_version}-full.zip -d . ; \
	chmod -R u=rwX,go=rX semanticturkey-${st_version} ; \
	chmod +x semanticturkey-${st_version}/bin/* ; \
	sed -i 's/\(data\.dir\)=.*/\1=..\/data\/SemanticTurkeyData/' /opt/pmki/semanticturkey-${st_version}/etc/it.uniroma2.art.semanticturkey.cfg ; \
	rm /tmp/pmki-${pmki_version}-full.zip

WORKDIR /opt/pmki/semanticturkey-$st_version

EXPOSE 1979

ENTRYPOINT ["bin/karaf"]
CMD ["server"]