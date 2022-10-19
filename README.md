# epics_make_loop_test
Basic IOC for testing infinite loop in Db/Makefile

The module was created using `makeBaseApp.pl` from base 7.0.7:
```
makeBaseApp.pl -t support loopTest
```
Then I did:
```
cd loopTestApp/Db/  
echo "# some database logic" > example.template
echo "include \"example.generated\"" >> example.template
```

The `Db/Makefile` was then edited to look like:
```
TOP=../..
include $(TOP)/configure/CONFIG

DB += example.db

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD RULES AFTER THIS LINE

example.tempate: example.generated

example.generated:
        echo "# some more database logic" > example.generated

```

I then compiled the `loopTestApp/Db` directory using these different versions of EPICS:
1. 3.14.12.8
2. 3.15.9
3. 7.0.7

Edit the `configure/RELEASE` to point to your local base installations.

Using 3.14.12.8:
```
[mkp@bl100-dassrv1 Db]$ mm
rm -rf O.linux-x86_64 O.Common
make: *** No rule to make target `uninstall'.  Stop.
perl /home/controls/epics/base/base-3.14.12.8/bin/linux-x86_64/makeMakefile.pl O.linux-x86_64 ../../..
mkdir O.Common
make -C O.linux-x86_64 -f ../Makefile TOP=../../.. \
    T_A=linux-x86_64 install
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
echo "# some more database logic" > example.generated
Inflating database from example.template
msi    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-3.14.12.8/db example.template > example.tmp
mv example.tmp ../O.Common/example.db
Installing created db file ../../../db/example.db
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
```

Using 3.15.9:
```
[mkp@bl100-dassrv1 Db]$ make
perl -CSD /home/controls/epics/base/base-3.15.9/bin/linux-x86_64/makeMakefile.pl O.linux-x86_64 ../../..
mkdir O.Common
make -C O.linux-x86_64 -f ../Makefile TOP=../../.. \
    T_A=linux-x86_64 install
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
echo "# some more database logic" > example.generated
/home/controls/epics/base/base-3.15.9/bin/linux-x86_64/msi -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-3.15.9/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
/home/controls/epics/base/base-3.15.9/bin/linux-x86_64/msi -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-3.15.9/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
/home/controls/epics/base/base-3.15.9/bin/linux-x86_64/msi -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-3.15.9/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
/home/controls/epics/base/base-3.15.9/bin/linux-x86_64/msi -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-3.15.9/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
/home/controls/epics/base/base-3.15.9/bin/linux-x86_64/msi -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-3.15.9/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
/home/controls/epics/base/base-3.15.9/bin/linux-x86_64/msi -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-3.15.9/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
/home/controls/epics/base/base-3.15.9/bin/linux-x86_64/msi -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-3.15.9/db -o ../O.Common/example.db example.template > example.db.d
^Cmake: *** [install.linux-x86_64] Interrupt
```

we got an inifinite loop.

Using 7.0.7:
```
[mkp@bl100-dassrv1 Db]$ make
perl -CSD /home/controls/epics/base/base-7.0.7/bin/linux-x86_64/makeMakefile.pl O.linux-x86_64 ../../..
mkdir -p O.Common
make -C O.linux-x86_64 -f ../Makefile TOP=../../.. \
    T_A=linux-x86_64 install
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
echo "# some more database logic" > example.generated
"/home/controls/epics/base/base-7.0.7/bin/linux-x86_64/msi" -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-7.0.7/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
"/home/controls/epics/base/base-7.0.7/bin/linux-x86_64/msi" -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-7.0.7/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
"/home/controls/epics/base/base-7.0.7/bin/linux-x86_64/msi" -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-7.0.7/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
"/home/controls/epics/base/base-7.0.7/bin/linux-x86_64/msi" -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-7.0.7/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
"/home/controls/epics/base/base-7.0.7/bin/linux-x86_64/msi" -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-7.0.7/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
"/home/controls/epics/base/base-7.0.7/bin/linux-x86_64/msi" -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-7.0.7/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
"/home/controls/epics/base/base-7.0.7/bin/linux-x86_64/msi" -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-7.0.7/db -o ../O.Common/example.db example.template > example.db.d
make[1]: Leaving directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
make[1]: Entering directory `/SNS/users/mkp/epics_make_loop_test/loopTestApp/Db/O.linux-x86_64'
"/home/controls/epics/base/base-7.0.7/bin/linux-x86_64/msi" -D    -I. -I.. -I../O.Common -I../../../db -I/home/controls/epics/base/base-7.0.7/db -o ../O.Common/example.db example.template > example.db.d
^Cmake: *** [install.linux-x86_64] Interrupt
```

same loop.

A workaround is to make this edit to the `Db/Makefile`:
```
diff --git a/loopTestApp/Db/Makefile b/loopTestApp/Db/Makefile
index befc80a..995fdf0 100644
--- a/loopTestApp/Db/Makefile
+++ b/loopTestApp/Db/Makefile
@@ -21,6 +21,6 @@ example.template: example.generated

 example.generated:
        echo "# some more database logic" > example.generated
-
+       cp ../example.template .

```

