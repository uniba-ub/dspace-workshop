#-include $(shell curl -sSL -o .build-harness "https://git.io/build-harness"; echo .build-harness)

### Git ###
ifneq ($(strip $(shell git status --porcelain 2>/dev/null)),)
	IS_DIRTY = -dirty
endif

#CI_PROJECT_URL ?=
CI_PROJECT_NAME ?= $(shell basename $(PWD))
CI_COMMIT_SHA ?= $(shell git rev-parse --short HEAD)$(IS_DIRTY)
CI_COMMIT_TAG ?= $(shell git describe --abbrev=0 --tags 2>/dev/null)

ifneq ("$(CI_COMMIT_TAG)","")
  TAG_REV = $(shell git rev-list -n 1 $(CI_COMMIT_TAG))
  COUNT_SINCE_TAG = $(shell git rev-list `git describe --abbrev=0 --tags`..HEAD --count)
endif

GITREMOTE := $(shell git config --get remote.origin.url)
GITPROT := $(shell echo "$(GITREMOTE)" | sed -r "s|((.*)(\:\/\/))?((.*)\@)?([^\/:]*)(\:([0-9]+))?[:\/](.*)(\.git)|\2|")
GITUSER := $(shell echo "$(GITREMOTE)" | sed -r "s|((.*)(\:\/\/))?((.*)\@)?([^\/:]*)(\:([0-9]+))?[:\/](.*)(\.git)|\5|")
GITHOST := $(shell echo "$(GITREMOTE)" | sed -r "s|((.*)(\:\/\/))?((.*)\@)?([^\/:]*)(\:([0-9]+))?[:\/](.*)(\.git)|\6|")
GITPORT := $(shell echo "$(GITREMOTE)" | sed -r "s|((.*)(\:\/\/))?((.*)\@)?([^\/:]*)(\:([0-9]+))?[:\/](.*)(\.git)|\8|")
GITPATH := $(shell echo "$(GITREMOTE)" | sed -r "s|((.*)(\:\/\/))?((.*)\@)?([^\/:]*)(\:([0-9]+))?[:\/](.*)(\.git)|\9|")

##############
### Docker ###
##############
#DOCKER_REPOSITORY ?= $(CI_PROJECT_NAME)
DOCKER_REPOSITORY ?= $(GITPATH)
DOCKER_TAG ?= local
VENDOR ?= ITFL-Service Uni Bamberg
MAINTAINER ?= ITFL-Service <itfl-service@uni-bamberg.de>
CI_REGISTRY_IMAGE ?= $(DOCKER_REGISTRY)$(DOCKER_REPOSITORY)
CI_PIPELINE_ID ?= $(DOCKER_TAG)

###############################
.EXPORT_ALL_VARIABLES:

.PHONY: all
all: clean docker_build ## Build binary and docker image

.PHONY: clean
clean: ## Cleanup backup files and compiled binaries
	@echo "Cleanup and *~"
	@find . -type f -name '*~' -delete
# Dry run
	git clean -dXn
# Interactive run
#	@git clean -dXi
# Force run
#	@git clean -dXf

.PHONY: docker_build
docker_build: ## Build docker image
	@echo "Building $(CI_PROJECT_NAME)"
	docker image build --pull \
	--label gitlab-pipeline-id="$(CI_PIPELINE_ID)" \
	--label maintainer="$(MAINTAINER)" \
	--label org.label-schema.schema-version="1.0" \
	--label org.label-schema.vendor="$(VENDOR)" \
	--label org.label-schema.vcs-ref="$(CI_COMMIT_SHA)" \
	--label org.label-schema.version="$(CI_COMMIT_TAG)" \
	--label org.label-schema.vcs-url="$(CI_PROJECT_URL)" \
	--label org.label-schema.name="$(CI_PROJECT_NAME)" \
	--label org.label-schema.build-date=`date -u +"%Y-%m-%dT%H:%M:%SZ_%Z"` \
	--tag $(CI_REGISTRY_IMAGE):$(CI_PIPELINE_ID) .

.PHONY: help
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' Makefile | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

###############################

.PHONY: repo_show
repo_show: ## Show some repository information
	@echo "Current Revision: $(CI_COMMIT_SHA)"
	@echo "Last Tag: $(CI_COMMIT_TAG)"
	@echo "Revision of last Tag: $(TAG_REV)"
	@echo "Count of commits since last Tag: $(COUNT_SINCE_TAG)"
	@git status --porcelain
	@echo "Commits since last tag:"
	@git log `git describe --tags --abbrev=0`..HEAD --oneline

	@echo "Loglevel $(ITFL_LOG_LEVEL)"

### docker ###

.PHONY: dc_up
dc_up: ## Start container
	@docker-compose config
	@docker-compose down -v || true
	@docker-compose pull --ignore-pull-failures
	@docker-compose up -d

.PHONY: dc_down
dc_down: ## Stop & cleanup container
	@docker-compose down -v

.PHONY: pdf
pdf: ## Create PDF
	@docker-compose run --rm -T -v $(PWD)/template/images:/slides/images web --verbose pdf -- --chrome-arg="--no-sandbox" slides.md
#	@docker container run --rm -v $(PWD):/data -w /data alpine:latest /bin/sh -c "chown -R $(UID): pdf/*"

.PHONY: slides
slides: ## Start presentation
	@docker-compose up -d
	@echo "Open in browser: http://127.0.0.1:4100"
