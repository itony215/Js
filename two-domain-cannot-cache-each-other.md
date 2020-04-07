# two domain cannot cache each other

worker cdn 

cache control



#### How can I configure CDN for Cloud Run services?

Since you can’t use [Cloud HTTP\(S\) Load Balancer](https://cloud.google.com/load-balancing/docs/https/) with Cloud Run, you cannot use [Cloud CDN](https://cloud.google.com/cdn/).

However, you can have CDN from [Firebase Hosting](https://firebase.google.com/docs/hosting/) by:

* responding to requests with a [`Cache-Control` header](https://firebase.google.com/docs/hosting/manage-cache#set_cache-control), and
* configuring a [rewrite configuration in `firebase.json`](https://firebase.google.com/docs/hosting/cloud-run#direct_requests_to_container) of your Firebase app.

[https://github.com/ahmetb/cloud-run-faq\#how-can-i-configure-cdn-for-cloud-run-services](https://github.com/ahmetb/cloud-run-faq#how-can-i-configure-cdn-for-cloud-run-services)



