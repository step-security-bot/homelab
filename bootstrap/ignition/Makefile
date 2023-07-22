BUS=$(wildcard *.bu)
IGNS=$(BUS:.bu=.ign)

INCLUDEBUS=$(wildcard includes/*.bu)
INCLUDEIGNS=$(INCLUDEBUS:.bu=.ign)

.DEFAULT_GOAL := help
.PHONY: help
help: ## Show this help
	$(info Includes: $(INCLUDEBUS))
	$(info Profiles: $(BUS))
	
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: includes
includes: $(INCLUDEIGNS) ## Compile includes

.PHONY: build
build: $(IGNS) ## Compile profiles

%.ign: %.bu
	$(info Compiling $*)
	@docker run --interactive --rm --security-opt label=disable \
       --volume ${PWD}:/pwd --workdir /pwd quay.io/coreos/butane:release \
       --files-dir . --pretty --strict $*.bu > $@ 

.PHONY: all
all: includes build ## Build all ignition files

.PHONY: clean
clean: 
	@rm -rf $(INCLUDEIGNS)
	@rm -rf $(IGNS)