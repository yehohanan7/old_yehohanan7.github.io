---
layout: post
title:  "Emacs behind proxy"
date:   2015-03-13
categories: emacs
---

<h4> <small> Emacs behind proxy </small></h4>

If you want to run emacs smoothly behind a proxy, you can put the below script in your init.el file and activate it by invoking proxy-activate from M-x

{% highlight elisp %}

(defun proxy-activate ()
  (interactive)

  (let ((proxy "host:port") (credentials "username:password"))
    (setq url-proxy-services
      `(("no_proxy" . "^\\(localhost\\|10.*\\)")
       ("http" . ,proxy)
       ("https" . ,proxy)))

    (setq url-http-proxy-basic-auth-storage
      (list (list proxy
                (cons "Input your LDAP UID !"
                      (base64-encode-string credentials)))))))

(provide 'proxy-activate)

{% endhighlight %}


