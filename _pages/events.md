---
permalink: /events/
title: "Events"
---

<script type="text/javascript">
const timezone = Intl.DateTimeFormat().resolvedOptions().timeZone 
const html = `<iframe src="https://calendar.google.com/calendar/embed?height=600&wkst=1&bgcolor=%23ffffff&mode=AGENDA&showNav=1&showCalendars=0&showTitle=0&src=ODhjYmI0NWJhNDdjMDk0Yjk0ZjFkNjg1MjJhMTQxZjQ5NTllZWRlMDFiMDNjYzQ1MzAyNzg0YTE0ODJlY2Y3ZkBncm91cC5jYWxlbmRhci5nb29nbGUuY29t&color=%23F09300&ctz=${timezone}" style=" border-width:0 " width="800" height="600" frameborder="0" scrolling="no"></iframe>`
document.getElementById('calendar-container').innerHTML = html;
</script>
<div id="calendar-container"></div>
