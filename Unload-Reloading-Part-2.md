

git archive --prefix=all/ raspberrypi-kernel_1.20210303-1:sound/soc/ > /tmp/all.tar.gz
git archive --prefix=drivers/gpio/ raspberrypi-kernel_1.20210303-1:drivers/gpio/ > /tmp/drivers-gpio.tar.gz

```
git init
git add .
git commit -m 'git archive --prefix=all/ raspberrypi-kernel_1.20210303-1:sound/soc/ > /tmp/all.tar.gz' -a
```

make -C /lib/modules/`uname -r`/build M=`pwd` clean

```
diff --git a/bcm/hifiberry_dacplus.c b/bcm/hifiberry_dacplus.c
index bdcac1b..85dd40b 100644
--- a/bcm/hifiberry_dacplus.c
+++ b/bcm/hifiberry_dacplus.c
@@ -18,7 +18,7 @@
 
 #include <linux/module.h>
 #include <linux/gpio/consumer.h>
-#include <../drivers/gpio/gpiolib.h>
+#include "gpiolib.h"
 #include <linux/platform_device.h>
 #include <linux/kernel.h>
 #include <linux/clk.h>
```

copy to `bcm/gpiolib.h`
