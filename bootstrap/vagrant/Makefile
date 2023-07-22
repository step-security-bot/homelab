OUTPUT := $(if $(OUTPUT_DIR),$(OUTPUT_DIR), ./output)
BOXES := fedora-coreos empty

.DEFAULT_GOAL := help
.PHONY: help

help: ## Show this help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: vagrant-box-builder
vagrant-box-builder:
	$(info Building vagrant-box builder)
	@docker buildx build --load --tag 'vagrant-box-builder:local' boxes/.

.PHONY: boxes
boxes: $(BOXES) ## Build and add all Vagrant boxes

$(BOXES): %: $(OUTPUT)/%.json
	$(info Adding vagrant-box $*)
	@cd $(OUTPUT)/; vagrant box add --force $*.json

$(OUTPUT)/:
	$(info Creating output directory $(OUTPUT))
	@mkdir -p $(OUTPUT)

$(OUTPUT)/%.json: ./boxes/%/ $(OUTPUT)/ vagrant-box-builder 
	$(info Building vagrant-box $*)
	@docker run --user root:root -v ./boxes/$*:/build -v $(OUTPUT):/output vagrant-box-builder:local

.PHONY: all
all: boxes 

.PHONY: clean
CLEANBOXES = $(addsuffix .clean,$(BOXES))

clean: $(OUTPUT)/  $(CLEANBOXES)
	$(info Cleaning output directory $(OUTPUT))
	@rm -rf $(OUTPUT)

$(CLEANBOXES): %.clean:
	$(info Cleaning vagrant-box $*)
	@docker run --user root:root -v ./boxes/$*:/build -v $(OUTPUT):/output vagrant-box-builder:local clean
	@-vagrant box remove $*
	