makefile_path := $(abspath $(firstword $(MAKEFILE_LIST)))
project_dir := $(abspath $(dir $(makefile_path)))

# Args
#slides=index.adoc
slides=*.adoc
destination=./build
theme=white # https://revealjs.com/themes/
highlighter_theme=a11y-light.css # https://github.com/highlightjs/highlight.js/tree/9-18-stable/src/styles

# RevealJS Options
# https://revealjs.com/config/
# https://docs.asciidoctor.org/reveal.js-converter/latest/converter/revealjs-options/

a_theme=-a revealjs_theme=${theme}
a_customcss=-a customcss=./css/tweaks.css
a_dimensions=-a revealjs_width=1920 -a revealjs_height=1080
a_controlsLayout=-a revealjs_controlsLayout=edges # Determines where controls appear, "edges" or "bottom-right"
a_navigationMode=-a revealjs_navigationMode=linear # Changes to linear navigation (left -right)
a_controls=-a revealjs_controls=true # Display presentation control arrows
a_slideNumber=-a revealjs_slideNumber=c/t # Display current over total slide number
a_overviewMode=-a revealjs_overviewMode=true # Enable the slide overview mode https://revealjs.com/overview/
a_transition=-a revealjs_transition=fade # Slide transition style (none/fade/slide/convex/concave/zoom)
a_backgroundTransition=-a revealjs_backgroundTransition=convex # Transition style for full page slide backgrounds
a_hash=-a revealjs_hash=true # Add the current slide number to the URL hash so that reloading the page/copying the URL will return you to the same slide
a_highlighter=-a source-highlighter=highlight.js
a_highlighter_theme=-a highlightjs-theme=css/${highlighter_theme}

# unused
a_revealjs_dir=-a revealjsdir=https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.1.2
a_allow_uri_read=--attribute allow-uri-read=true

attributes=\
  -a toc=left \
  -a icons=font \
  -a imagesdir=./images/ \
  ${a_theme} \
  ${a_customcss} \
  ${a_dimensions} \
  ${a_controlsLayout} \
  ${a_navigationMode} \
  ${a_hash} \
  ${a_controls} \
  ${a_slideNumber} \
  ${a_overviewMode} \
  ${a_transition} \
  ${a_backgroundTransition} \
  ${a_highlighter} \
  ${a_highlighter_theme}


CMD=asciidoctor-revealjs \
  --require asciidoctor-diagram \
  --destination-dir ${destination} \
  ${attributes} \
  "${slides}"


ASCIIDOCTOR_TOOLCHAIN=asciidoctor/docker-asciidoctor


REVEALJS_DIR=${destination}/reveal.js



.PHONY: build

.DEFAULT_GOAL := help

help:
	@echo "Args"
	@echo "  slides=${slides}"
	@echo "  destination=${destination}"
	@echo "  makefile=${makefile_path}"
	@echo ""
	@echo "Makefile commands:"
	@echo "  help"
	@echo "  build"
	@echo "  all"

clean:
	@rm -rf ${destination}

build: resources
	@echo "Running:"
	@echo "  ${CMD}"
	@docker run --rm --volume ${project_dir}:/documents ${ASCIIDOCTOR_TOOLCHAIN} ${CMD}

all: help build

resources: $(REVEALJS_DIR) highlightjs
	@cp -r images css ${destination}

$(REVEALJS_DIR):
	@git clone -b 3.9.2 --depth 1 https://github.com/hakimel/reveal.js.git $(REVEALJS_DIR) 2> /dev/null || (cd $(REVEALJS_DIR) ; git pull)

highlightjs: ${highlighter_theme}

${highlighter_theme}:
	@curl --silent \
      --remote-time\
	  --location \
	  --output ${destination}/css/${highlighter_theme} \
	  https://github.com/highlightjs/highlight.js/raw/9-18-stable/src/styles/${highlighter_theme}