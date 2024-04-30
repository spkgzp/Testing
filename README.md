cat /etc/haproxy/haproxy.cfg
global
  #log         127.0.0.1 local2
  pidfile     /var/run/haproxy.pid
  maxconn     4000
  daemon
defaults
  mode                    http
  log                     global
  option                  dontlognull
  option http-server-close
  option                  redispatch
  retries                 3
  timeout http-request    10s
  timeout queue           1m
  timeout connect         10s
  timeout client          1m
  timeout server          1m
  timeout http-keep-alive 10s
  timeout check           10s
  maxconn                 3000
frontend stats
  bind *:1936
  mode            http
  log             global
  maxconn 10
  stats enable
  stats hide-version
  stats refresh 30s
  stats show-node
  stats show-desc Stats for ocp4 cluster 
  stats auth admin:ocp4
  stats uri /stats
listen api-server-6443 
  bind *:6443
  mode tcp
#  server bootstrap bootstrap.ocpuat.finopaymentbank.in:6443 check inter 1s backup 
  server master0 master1.ocpuat.finopaymentbank.in:6443 check inter 1s
  server master1 master2.ocpuat.finopaymentbank.in:6443 check inter 1s
  server master2 master3.ocpuat.finopaymentbank.in:6443 check inter 1s
listen machine-config-server-22623 
  bind *:22623
  mode tcp
#  server bootstrap bootstrap.ocpuat.finopaymentbank.in:22623 check inter 1s backup 
  server master0 master1.ocpuat.finopaymentbank.in:22623 check inter 1s
  server master1 master2.ocpuat.finopaymentbank.in:22623 check inter 1s
  server master2 master3.ocpuat.finopaymentbank.in:22623 check inter 1s

listen ingress-router-443 
  bind *:443
  mode tcp
  balance roundrobin
  server worker1 worker1.ocpuat.finopaymentbank.in:443 check inter 1s
  server worker2 worker2.ocpuat.finopaymentbank.in:443 check inter 1s
  server worker3 worker3.ocpuat.finopaymentbank.in:443 check inter 1s
  server worker4 worker4.ocpuat.finopaymentbank.in:443 check inter 1s
  server worker5 worker5.ocpuat.finopaymentbank.in:443 check inter 1s
  server worker6 worker6.ocpuat.finopaymentbank.in:443 check inter 1s
  server worker7 worker7.ocpuat.finopaymentbank.in:443 check inter 1s
  server worker8 worker8.ocpuat.finopaymentbank.in:443 check inter 1s

listen ingress-router-80
  bind *:80
  mode http
  balance roundrobin
  server worker1 worker1.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker2 worker2.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker3 worker3.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker4 worker4.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker5 worker5.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker6 worker6.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker7 worker7.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker8 worker8.ocpuat.finopaymentbank.in:80 check inter 1s



listen ingress-router-9002 
  bind *:9002
  balance roundrobin
  mode http
  option forwardfor
  ##http-request set-header Host bll-apigateway.apps.ocpuat.finopaymentbank.in
  #reqirep ^Host: Host:\ bll-apigateway.apps.ocpuat.finopaymentbank.in
  server worker1 worker1.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker2 worker2.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker3 worker3.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker4 worker4.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker5 worker5.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker6 worker6.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker7 worker7.ocpuat.finopaymentbank.in:80 check inter 1s
  server worker8 worker8.ocpuat.finopaymentbank.in:80 check inter 1s
