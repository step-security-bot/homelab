.DEFAULT_GOAL := help
.PHONY: help
help: ## Show this help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: vagrant
vagrant: ignition ## Install vagrant boxes and plugins 
	@cd vagrant; make all

.PHONY: ignition
ignition: ## Make ignition files
	@cd ignition; make all

.PHONY: clean
clean: ## Clean up 
	@cd ignition; make clean
	@cd vagrant; make clean