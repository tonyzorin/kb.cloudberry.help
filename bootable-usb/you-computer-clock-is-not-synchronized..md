# You computer clock is not synchronized.

## Problem

When trying to restore image-based backup via bootable USB device, you may receive an error "_Your computer clock is not synchronized. For security reasons, Amazon generates error when your clock is not synchronized. Check out http://www.greenwichmeantime.com/ to synchronize your clock._". This may be a cause of incorrect local time or unavailable network time server.

![](../.gitbook/assets/image.png)

## Suggestions and Resolution

### 1. Check computer time and date

Go to Boot Menu -&gt; Tools -&gt; Command prompt. Check time and date by respective commands "time" and "date". You can adjust time and date by the same commands. 

![Use commands &quot;date&quot; and &quot;time&quot;](../.gitbook/assets/image%20%281%29.png)

### 2. Sync computer time with network time server

Go to Boot Menu -&gt; Tools -&gt; press key "t". Your computer will try to sync time and date with time server "time.windows.com". Successful synchronization result is "Done".

![Successful time synchronization](../.gitbook/assets/image%20%282%29.png)

{% hint style="warning" %}
If, as a result of time synchronization, your computer received "_A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host failed to respond_", you should check that UDP port 123 allowed in NAT rules.
{% endhint %}

![Failed time synchronization](../.gitbook/assets/image%20%283%29.png)

