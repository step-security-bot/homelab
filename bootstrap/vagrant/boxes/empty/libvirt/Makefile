OUTPUT := ${OUTPUT_DIR}

$(OUTPUT)/empty-libvirt.box: box.img metadata.json Vagrantfile | $(OUTPUT)/
	tar czvf $@ $^

$(OUTPUT)/:
	mkdir -p $(OUTPUT)

box.img:
	qemu-img create -f qcow2 box.img 10G

.PHONY: clean
clean:
	@rm -rf box.img
	@rm -rf $(OUTPUT)/empty-libvirt.box