
icon_in_files = $(shell SRCDIR="$(srcdir)"; for i in `find $$SRCDIR -name "*.svg"`; do printf "$$i "; done)

if ALLOW_RENDERING
render-png: $(icon_in_files) Makefile
	for file in $(icon_in_files); do \
		for size in $(render_sizes); do \
			$(ICONTOOL_RENDER) -s $$size -o $(top_builddir)/$(SVGOUTDIR) $$file >/dev/null; \
		done; \
	done

else
render-png: $(icon_in_files) Makefile
	if test "x$(top_srcdir)" != "x$(top_builddir)"; then \
		cp -a $(top_srcdir)/$(SVGOUTDIR) $(top_builddir)/$(SVGOUTDIR); \
	fi

endif

$(render_sizes): Makefile

render: render-png

install-data-local:
	for size in $(render_sizes); do \
		echo -e "Going to copy files for $$size"; \
		for file in `cd $(top_srcdir)/$(SVGOUTDIR)/$$size && find . -name "*.png"`; do \
			context="`dirname $$file`"; \
			$(mkdir_p) $(DESTDIR)$(themedir)/$$size/$$context; \
			$(install_sh_DATA) $(top_srcdir)/$(SVGOUTDIR)/$$size/$$file $(DESTDIR)$(themedir)/$$size/$$file; \
		done; \
	done; \
	if [ $(ICONMAP) != "false" ]; then \
		cd $(DESTDIR)$(themedir); \
		for size in $(render_sizes); do \
			cd $$size || continue; \
			echo -e "Going to set up icon mapping for $$size"; \
			for dir in `find . -type d`; do \
				context="`echo $$dir | cut -c 3-`"; \
				if [ $$context ]; then \
					$(ICONMAP) -c $$context; \
				fi; \
			done; \
			cd ..; \
		done; \
	fi

## FIXME we should add a way to remove links generated by icon mapping
uninstall-local:
	for size in $(render_sizes); do \
		for file in `cd $(top_srcdir)/$(SVGOUTDIR)/$$size && find . -name "*.png"`; do \
			rm -f $(DESTDIR)$(themedir)/$$size/$$file; \
		done; \
	done

EXTRA_DIST =			\
	$(icon_in_files)


MAINTAINERCLEANFILES =		\
	Makefile.in		\
	$(icon_cache)
