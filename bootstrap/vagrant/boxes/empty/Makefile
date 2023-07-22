OUTPUT := ${OUTPUT_DIR}

$(OUTPUT)/empty.json: $(OUTPUT)/empty-libvirt.box
	$(shell jq --arg libvirt $(shell sha256sum $(word 1,$^) | cut -d " " -f 1) \
		'.versions[0].providers[0].checksum = $$libvirt | \
		.versions[0].providers[0].checksum_type = "sha256"' \
		info-template.json > $@)

$(OUTPUT)/empty-libvirt.box: $(OUTPUT)/
	@cd libvirt; make 

$(OUTPUT)/:
	mkdir -p $(OUTPUT)

.PHONY: clean
clean:
	@cd libvirt; make clean
	rm -f $(OUTPUT)/empty.json