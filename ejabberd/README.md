The `snapcraft.yaml` does not quite allow us to create a working Snap package
yet.  Instead, you will need to create the prepared `snap` directory with
`snapcraft prime` and then apply the following patch:

```
diff -rNu --exclude '*.beam' ../../prime/sbin/ejabberdctl prime/sbin/ejabberdctl
--- ../../prime/sbin/ejabberdctl	2016-06-08 09:53:55.718037456 +0000
+++ prime/sbin/ejabberdctl	2016-06-06 23:42:56.656518496 +0000
@@ -7,15 +7,15 @@
 ERL_PROCESSES=250000
 ERL_MAX_ETS_TABLES=1400
 FIREWALL_WINDOW=""
-ERLANG_NODE=ejabberd@localhost
+ERLANG_NODE=ejabberd
 
 # define default environment variables
 SCRIPT_DIR=`cd ${0%/*} && pwd`
-ERL=/home/jtait/src/snappy-stuff/snappy-playpen/ejabberd/parts/ejabberd/install/usr/bin/erl
-IEX=/bin/iex
-EPMD=/home/jtait/src/snappy-stuff/snappy-playpen/ejabberd/parts/ejabberd/install/usr/bin/epmd
+ERL=$SNAP/usr/bin/erl
+IEX=$SNAP/bin/iex
+EPMD=$SNAP/usr/bin/epmd
 INSTALLUSER=
-ERL_LIBS=/lib
+ERL_LIBS=$SNAP/lib
 
 # check the proper system user is used if defined
 if [ "$INSTALLUSER" != "" ] ; then
diff -rNu --exclude '*.beam' ../../prime/usr/lib/erlang/bin/erl prime/usr/lib/erlang/bin/erl
--- ../../prime/usr/lib/erlang/bin/erl	2016-04-08 10:19:33.000000000 +0000
+++ prime/usr/lib/erlang/bin/erl	2016-06-03 17:19:59.897833162 +0000
@@ -18,7 +18,7 @@
 # 
 # %CopyrightEnd%
 #
-ROOTDIR=/usr/lib/erlang
+ROOTDIR=$SNAP/usr/lib/erlang
 BINDIR=$ROOTDIR/erts-7.3/bin
 EMU=beam
 PROGNAME=`echo $0 | sed 's/.*\///'`
```

Then you can create the Snap package with `snapcraft snap`.
