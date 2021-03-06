#Makefile for Rainloop

#Default build command
all: clean setup

#Must be run before build!
.PHONY: clean
clean:
	#cleanAssets
	rm -rf rainloop/v/0.0.0/static/
	mkdir rainloop/v/0.0.0/static/
	#cleanCSS
	rm -f rainloop/v/0.0.0/static/css/*.css
	mkdir rainloop/v/0.0.0/static/css
	#cleanJS
	rm -f rainloop/v/0.0.0/static/js/**/*.js
	mkdir rainloop/v/0.0.0/static/js
	mkdir rainloop/v/0.0.0/static/js/min
	#cleanMoment
	rm -f rainloop/v/0.0.0/app/localization/moment/*.js
	#cleanLightgalleryFonts
	mkdir rainloop/v/0.0.0/static/css/fonts
	rm -f rainloop/v/0.0.0/static/css/fonts/lg.*
	#cleanFontasticFonts
	rm -f rainloop/v/0.0.0/static/css/fonts/rainloop.*
	#cleanOpenPGP
	rm -f rainloop/v/0.0.0/static/js/min/openpgp.*
	#cleanCKEditor
	rm -rf rainloop/v/0.0.0/static/ckeditor-plugins/
	mkdir rainloop/v/0.0.0/static/ckeditor-plugins
	#cleanDist
	rm -rf build/dist/

#Assets
.PHONY: assets
assets:
	cp -r assets/* rainloop/v/0.0.0/static/





#CSS
cssMainCSS = \
	/usr/lib/nodejs/normalize.css/normalize.css \
	vendors/jquery-ui/css/smoothness/jquery-ui-1.10.3.custom.css \
	vendors/fontastic/styles.css \
	vendors/jquery-nanoscroller/nanoscroller.css \
	vendors/jquery-letterfx/jquery-letterfx.css \
	vendors/inputosaurus/inputosaurus.css \
	vendors/flags/flags-fixed.css \
	/usr/lib/nodejs/opentip/css/opentip.css \
	/usr/lib/nodejs/pikaday/css/pikaday.css \
	/usr/lib/nodejs/lightgallery/dist/css/lightgallery.css \
	/usr/lib/nodejs/lightgallery/dist/css/lg-transitions.css \
	/usr/lib/nodejs/progressjs/src/progressjs.css \
	dev/Styles/_progressjs.css

cssMainLess = dev/Styles/@Main.less

cssSocialCSS = \
	vendors/fontastic/styles.css \
	dev/Styles/_social.css

.PHONY: cssMain
cssMain: $(cssMainCSS)
	sed "s/\.\.\/(img|images|fonts|svg)\//$1/g" $^ \
	> rainloop/v/0.0.0/static/css/app.css

.PHONY: cssMainLess
cssMainLess: $(cssMainLess) | cssMain
	lessc --include-path="dev/Styles/:vendors/bootstrap/less/" $< \
	| sed "s/\.\.\/(img|images|fonts|svg)\//$1/g" \
	>> rainloop/v/0.0.0/static/css/app.css

.PHONY: cssSocial
cssSocial: $(cssSocialCSS)
	sed "s/\.\.\/(img|images|fonts|svg)\//$1/g" $^ \
	> rainloop/v/0.0.0/static/css/social.css

.PHONY: cssMainMin
cssMainMin: | cssMainLess
	cleancss -o rainloop/v/0.0.0/static/css/app.min.css rainloop/v/0.0.0/static/css/app.css

.PHONY: cssSocialMin
cssSocialMin: | cssSocial
	cleancss -o rainloop/v/0.0.0/static/css/social.min.css rainloop/v/0.0.0/static/css/social.css

.PHONY:
cssMin: | cssMainMin cssSocialMin





#JS
header = "/* RainLoop Webmail (c) RainLoop Team | Licensed under AGPL v3 */"

jsLibs = \
	/usr/lib/nodejs/jquery/dist/jquery.js \
	/usr/share/javascript/jquery-mousewheel/jquery.mousewheel.js \
	/usr/share/javascript/jquery-lazyload/jquery.lazyload.js \
	/usr/share/javascript/jquery-backstretch/jquery.backstretch.js \
	vendors/jquery-ui/js/jquery-ui-1.10.3.custom.js \
	vendors/jquery-nanoscroller/jquery.nanoscroller.js \
	vendors/jquery-wakeup/jquery.wakeup.js \
	vendors/jquery-letterfx/jquery-letterfx.js \
	vendors/inputosaurus/inputosaurus.js \
	vendors/routes/signals.min.js \
	vendors/routes/hasher.min.js \
	vendors/routes/crossroads.min.js \
	vendors/jua/jua.js \
	vendors/keymaster/keymaster.js \
	vendors/qr.js/qr.js \
	vendors/bootstrap/js/bootstrap.js \
	/usr/share/javascript/underscore/underscore.js \
	/usr/share/javascript/moment/moment.js \
	/usr/lib/nodejs/knockout/build/output/knockout-latest.js \
	/usr/lib/nodejs/knockout-transformations/dist/knockout-transformations.js \
	/usr/lib/nodejs/knockout-sortable/build/knockout-sortable.js \
	/usr/lib/nodejs/simplestatemanager/dist/ssm.min.js \
	/usr/lib/nodejs/autolinker/dist/Autolinker.min.js \
	/usr/lib/nodejs/opentip/lib/opentip.js \
	/usr/lib/nodejs/opentip/lib/adapter-jquery.js \
	/usr/lib/nodejs/lightgallery/dist/js/lightgallery.js \
	/usr/lib/nodejs/lightgallery/dist/js/lg-fullscreen.js \
	/usr/lib/nodejs/lightgallery/dist/js/lg-thumbnail.js \
	/usr/lib/nodejs/lightgallery/dist/js/lg-zoom.js \
	/usr/lib/nodejs/lightgallery/dist/js/lg-autoplay.js \
	/usr/lib/nodejs/ifvisible.js/ifvisible.js

.PHONY: jsLibs
jsLibs: $(jsLibs)
	touch rainloop/v/0.0.0/static/js/libs.js
	for f in $^; do \
		sed "s/.*sourceMappingURL=.+//ig" $$f >> rainloop/v/0.0.0/static/js/libs.js; \
		printf "\n\n" >> rainloop/v/0.0.0/static/js/libs.js; \
	done

.PHONY: jsWebpack
jsWebpack:
	webpack

.PHONY: jsApp
jsApp: | jsWebpack
	mv rainloop/v/0.0.0/static/js/app.js rainloop/v/0.0.0/static/js/app.js.tmp
	echo $(header) > rainloop/v/0.0.0/static/js/app.js
	cat rainloop/v/0.0.0/static/js/app.js.tmp > rainloop/v/0.0.0/static/js/app.js
	rm rainloop/v/0.0.0/static/js/app.js.tmp

.PHONY: jsAdmin
jsAdmin: | jsWebpack
	mv rainloop/v/0.0.0/static/js/admin.js rainloop/v/0.0.0/static/js/admin.js.tmp
	echo $(header) > rainloop/v/0.0.0/static/js/admin.js
	cat rainloop/v/0.0.0/static/js/admin.js.tmp > rainloop/v/0.0.0/static/js/admin.js
	rm rainloop/v/0.0.0/static/js/admin.js.tmp

.PHONY: jsMin
jsMin: | jsApp jsAdmin jsLibs
	for f in app admin libs boot; do \
		sed "s/rainloop\/v\/([^\/]+)\/static\/js\//rainloop\/v\/$1\/static\/js\/min\//g" rainloop/v/0.0.0/static/js/$$f.js \
		| uglifyjs --compress --mangle --screw-ie8 -o rainloop/v/0.0.0/static/js/min/$$f.min.js; \
	done





#Vendors
.PHONY: moment
moment: | clean
	cp /usr/share/javascript/moment/locale/*.js rainloop/v/0.0.0/app/localization/moment/

.PHONY: lightgallery
lightgallery: | clean
	cp /usr/lib/nodejs/lightgallery/dist/fonts/lg.* rainloop/v/0.0.0/static/css/fonts/

.PHONY: fontastic
fontastic: | clean
	cp vendors/fontastic/fonts/rainloop.* rainloop/v/0.0.0/static/css/fonts/

.PHONY: ckeditorPlugins
ckeditorPlugins:
	cp -r vendors/ckeditor-plugins/plain rainloop/v/0.0.0/static/ckeditor-plugins/
	cp -r vendors/ckeditor-plugins/signature rainloop/v/0.0.0/static/ckeditor-plugins/
	cp -r vendors/ckeditor/skins/moono-lisa rainloop/v/0.0.0/static/ckeditor-plugins/

.PHONY: vendors
vendors: | moment ckeditorPlugins fontastic lightgallery




#Dist
#Version from package.json
VERSION = 1.11.1

.PHONY: copy
copy: | assets jsMin cssMin vendors
	mkdir build/dist/
	mkdir build/dist/rainloop/
	mkdir build/dist/data/
	cp -r rainloop/v/0.0.0/* build/dist/rainloop
	find build/dist/rainloop/ -type d -exec chmod 0755 {} +
	find build/dist/rainloop/ -type f -exec chmod 0644 {} +
	#Remove libraries already in Debian
	rm -rf build/dist/rainloop/app/libraries/pclzip/
	rm -rf build/dist/rainloop/app/libraries/GuzzleHttp/
	rm -rf build/dist/rainloop/app/libraries/phpseclib/
	rm -rf build/dist/rainloop/app/libraries/Predis/
	#Remove VCS control files for upstream bundled libraries
	rm -f build/dist/rainloop/app/libraries/Mobile_Detect/.gitignore
	rm -f build/dist/rainloop/app/libraries/Mobile_Detect/.gitmodules
	#Remove extra license files for upstream bundled libraries
	rm -f build/dist/rainloop/app/libraries/MailSo/LICENSE
	rm -f build/dist/rainloop/app/libraries/Mobile_Detect/LICENSE.txt
	rm -f build/dist/rainloop/app/libraries/lessphp/LICENSE
	rm -f build/dist/rainloop/app/libraries/spyc/COPYING
	rm -f build/dist/rainloop/app/libraries/tmhOAuth/LICENSE

.PHONY: setup
setup: | copy
	echo "$(VERSION)" > build/dist/data/VERSION
	echo "$(VERSION)" > build/dist/data/EMPTY
	cp index.php build/dist/rainloop/index.php
	sed -i "s/'APP_VERSION', '0.0.0'/'APP_VERSION', '$(VERSION)'/" build/dist/rainloop/index.php
	sed -i "s/'APP_VERSION_TYPE', 'source'/'APP_VERSION_TYPE', 'community'/" build/dist/rainloop/index.php
	rm -f build/dist/rainloop/app/libraries/RainLoop/Providers/Prem.php
