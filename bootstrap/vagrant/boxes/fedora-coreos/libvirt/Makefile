OUTPUT := ${OUTPUT_DIR}
STREAM := stable
ARCH := x86_64
VERSION := $(shell curl -s "https://builds.coreos.fedoraproject.org/prod/streams/$(STREAM)/builds/builds.json" | \
	jq -r --arg arch "$(ARCH)" 'first(.builds[] | select(.arches[] | contains($$arch))) | .id // empty')
URL := "https://builds.coreos.fedoraproject.org/prod/streams/$(STREAM)/builds/$(VERSION)/$(ARCH)/fedora-coreos-$(VERSION)-qemu.$(ARCH).qcow2.xz"

$(OUTPUT)/fedora-coreos-libvirt.box: box.img metadata.json Vagrantfile | $(OUTPUT)/
	tar czvf $@ $^

$(OUTPUT)/:
	mkdir -p $(OUTPUT)

box.img: fedora-coreos-qemu.qcow2.xz
	xz -d -c $< > $@

fedora-coreos-qemu.qcow2.xz:
	curl -Lo $@ $(URL)

.PHONY: clean
clean:
	-rm -f box.img fedora-coreos-qemu.img fedora-coreos-qemu.qcow2.xz
	-rm -f $(OUTPUT)/fedora-coreos-libvirt.box