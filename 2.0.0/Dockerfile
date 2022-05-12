FROM adoptopenjdk/openjdk8:alpine

ENV showvoc_version=2.0.0
ENV st_version=11.0

COPY showvoc-${showvoc_version}-full.zip /tmp
RUN set -eux ; \
	mkdir /opt/showvoc ; \
	mkdir /opt/showvoc/data ; \
	cd /opt/showvoc ; \
	unzip -q /tmp/showvoc-${showvoc_version}-full.zip -d . ; \
	chmod -R u=rwX,go=rX semanticturkey-${st_version} ; \
	chmod +x semanticturkey-${st_version}/bin/* ; \
	sed -i 's/\(data\.dir\)=.*/\1=..\/data\/SemanticTurkeyData/' /opt/showvoc/semanticturkey-${st_version}/etc/it.uniroma2.art.semanticturkey.cfg ; \
	rm /tmp/showvoc-${showvoc_version}-full.zip

WORKDIR /opt/showvoc/semanticturkey-$st_version

EXPOSE 1979

ENTRYPOINT ["bin/karaf"]
CMD ["server"]