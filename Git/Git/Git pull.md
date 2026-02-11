git pull = [[Git fetch]] + [[Git merge]]


___
برای گرفتن آخرین تغییرات از یک ریموت در گیت، دستور کلی اینه:

`
git pull <remote> <branch>
`

پس اگر ریموتت origin هست و شاخه‌ات main، دقیقا همین میشه:

`
git pull origin main
`

اما چند نکتهٔ مهم:

🔹 اگر روی همان برنچی هستی که می‌خواهی pull کنی
مثلاً الان روی main هستی، کافی است بنویسی:

`
git pull
`

چون گیت خودش می‌داند upstream branch چیست.

🔹 اگر upstream تنظیم نشده باشد
می‌توانی یک بار این را تنظیم کنی:

`
git branch --set-upstream-to=origin/main
`

بعد از آن فقط:

`
git pull
`

اگر خواستی، می‌توانم تفاوت git pull و git fetch را هم توضیح بدهم یا کمک کنم upstream را درست تنظیم کنی.