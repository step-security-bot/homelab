OUTPUT := ${OUTPUT_DIR}
STREAM := stable
ARCH := x86_64
VERSION := $(shell curl -s "https://builds.coreos.fedoraproject.org/prod/streams/$(STREAM)/builds/builds.json" | \
	jq -r --arg arch "$(ARCH)" 'first(.builds[] | select(.arches[] | contains($$arch))) | .id // empty')

$(OUTPUT)/fedora-coreos.json: $(OUTPUT)/fedora-coreos-libvirt.box
	$(shell jq --arg version $(VERSION) \
	    --arg libvirt $(shell sha256sum $(word 1,$^) | cut -d " " -f 1) \
		'.versions[0].version = $$version | \
		.versions[0].providers[0].checksum = $$libvirt | \
		.versions[0].providers[0].checksum_type = "sha256"' \
		info-template.json > $@)

$(OUTPUT)/fedora-coreos-libvirt.box: 
	@cd libvirt; make


$(OUTPUT)/:
	mkdir -p $(OUTPUT)


.PHONY: clean
clean:
	@cd libvirt; make clean
	rm -f $(OUTPUT)/fedora-coreos.json