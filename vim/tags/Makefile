
FIND = /usr/bin/find
CTAGS = /usr/bin/ctags --c++-kinds=+p --c-kinds=+px-n --fields=+iatfS --extra=+q -I __wur,__THROW,__nonnull+
CSCOPE = /usr/bin/cscope -bkq

# all:libc.tags libqt4.tags libgtk.tags kernel.tags;
all:libc.tags kernel.tags;

libc.tags:
	dpkg -L libc6-dev | grep '\.h$$' > $(basename $@).list
	dpkg -L linux-libc-dev | grep '\.h$$' >> $(basename $@).list
	$(FIND) /usr/include/c++/ -type f >> $(basename $@).list
	sed -i '/bits\//d' $(basename $@).list
	uniq $(basename $@).list > temp.list
	mv temp.list $(basename $@).list
	$(CTAGS) -L $(basename $@).list -f $(basename $@).tags
	$(CSCOPE) -i$(basename $@).list -f$(basename $@).out

libqt4.tags:
	$(FIND) /usr/include/qt4/ -type f > $(basename $@).list
	uniq $(basename $@).list > temp.list
	mv temp.list $(basename $@).list
	$(CTAGS) -L $(basename $@).list -f $(basename $@).tags
	$(CSCOPE) -i$(basename $@).list -f$(basename $@).out

libgtk.tags:
	for heads in `pkg-config gtk+-2.0 --cflags`; do \
		[ "$${heads::2}" = "-I" ] && \
		$(FIND) "$${heads:2}" -type f -name '*.h' >> $(basename $@).list; \
		done
	uniq $(basename $@).list > temp.list
	mv temp.list $(basename $@).list
	$(CTAGS) -L $(basename $@).list -f $(basename $@).tags
	$(CSCOPE) -i$(basename $@).list -f$(basename $@).out

kernel.tags:
	$(FIND) $(realpath /lib/modules/$(shell uname -r)/build) -type f -name *.h > $(basename $@).list
	$(FIND) $(realpath /lib/modules/$(shell uname -r)/source) -type f -name *.h >> $(basename $@).list
	$(CTAGS) -L $(basename $@).list -f $(basename $@).tags
	$(CSCOPE) -i$(basename $@).list -f$(basename $@).out

clean:
	$(RM) *.list *.out *.tags *.in *.po
