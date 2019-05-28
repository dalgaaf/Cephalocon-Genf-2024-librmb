<!-- .slide: data-state="section-break" id="section-break-4" data-timing="10s" -->
# Dovecot and Ceph


<!-- .slide: data-state="normal" id="librmb-DT" data-timing="20s" data-menu-title="DT's approach" -->
## DT's approach
<div>
     <img style="position: absolute; width:30%; left: 63%;" alt="Partner"
          data-src="images/partner.png" />
</div> <!-- .element class="fragment" data-fragment-index="4"-->

* no open source solution on the market <!-- .element class="fragment" data-fragment-index="0"-->
* closed source or licence no option <!-- .element class="fragment" data-fragment-index="1"-->
* develop / sponsor a solution <!-- .element class="fragment" data-fragment-index="2"-->
* open source it <!-- .element class="fragment" data-fragment-index="3"-->
* partner with: <!-- .element class="fragment" data-fragment-index="4"-->
  * `Wido den Hollander (42on.com)` <!-- .element class="fragment" data-fragment-index="4"-->
  * `Tallence AG` for development <!-- .element class="fragment" data-fragment-index="4"-->
  * `SUSE` for Ceph <!-- .element class="fragment" data-fragment-index="4"-->

Note: 
- Dovecot Pro licence no option due to TCO impact. Model is to pay per account if active or inactive. With 39m accounts it would break the BC.


<!-- .slide: data-state="normal" id="librmb-DT-0" data-timing="20s" data-menu-title="Dovecot obox" -->
## Dovecot Pro obox Plugin

<div>
     <img style="width:90%" alt="obox architecture overview"
          data-src="images/dovecot-obox-plugin-architecture-normal.svg" />
</div>

Note: quite complex setup with many layers of caches, mobox is similar


<!-- .slide: data-state="normal" id="librmb-DT-1" data-timing="20s" data-menu-title="Ceph Dovecot Plugin" -->
## Ceph plugin for Dovecot
### First Step: hybrid approach <!-- .element class="fragment" data-fragment-index="0"-->

### Emails <!-- .element class="fragment" data-fragment-index="1"-->
* RADOS <!-- .element class="fragment" data-fragment-index="1"-->

### Metadata and indexes <!-- .element class="fragment" data-fragment-index="2"-->
* CephFS <!-- .element class="fragment" data-fragment-index="2"-->

### Generic email abstraction on top of librados <!-- .element class="fragment" data-fragment-index="4"-->
* Split code into libraries <!-- .element class="fragment" data-fragment-index="4"-->
* Give code back to corresponding upstream projects <!-- .element class="fragment" data-fragment-index="4"-->

Note: out of scope - user data and credential storage; full text search


<!-- .slide: data-state="normal" id="librmb-DT-2.1" data-timing="20s" data-menu-title="librmb" -->
## Librados mailbox (librmb)

<div>
     <img style="width:90%" alt="librmb architecture overview"
          data-src="images/dovecot-plugin-architecture-normal.svg" />
</div>


<!-- .slide: data-state="normal" id="librmb-DT-2.2" data-timing="20s" data-menu-title="librmb - Mail Object Format" -->
## librmb - Mail Object Format

### Mails are immutable regarding the RFC-5322 content <!-- .element: class="fragment" data-fragment-index="1" -->

### RFC-5322 content stored in RADOS directly <!-- .element: class="fragment" data-fragment-index="2" -->

<span class="fragment" data-fragment-index="3">
### Immutable attributes used by Dovecot stored in RADOS xattr <!-- .element: class="fragment" data-fragment-index="3" -->
* rbox format version <!-- .element: class="fragment" data-fragment-index="3" -->
* GUID <!-- .element: class="fragment" data-fragment-index="3" -->
* Received and save date <!-- .element: class="fragment" data-fragment-index="3" -->
* POP3 UIDL and POP3 order <!-- .element: class="fragment" data-fragment-index="3" -->
* Mailbox GUID <!-- .element: class="fragment" data-fragment-index="3" -->
* Physical and virtual size <!-- .element: class="fragment" data-fragment-index="3" -->
* Mail UID <!-- .element: class="fragment" data-fragment-index="3" -->
</span>

### writable attributes are stored in Dovecot index files <!-- .element: class="fragment" data-fragment-index="4" -->


<!-- .slide: data-state="normal" id="librmb-DT-2.3" data-timing="20s" data-menu-title="rmb tool" -->
## Dump email details from RADOS

```bash
$> rmb -p mail_storage -N t1 ls M=ad54230e65b49a59381100009c60b9f7

mailbox_count: 1

MAILBOX: M(mailbox_guid)=ad54230e65b49a59381100009c60b9f7
         mail_total=2, mails_displayed=2
         mailbox_size=5539 bytes

         MAIL:   U(uid)=4
                 oid = a2d69f2868b49a596a1d00009c60b9f7
                 R(receive_time)=Tue Jan 14 00:18:11 2003
                 S(save_time)=Mon Aug 21 12:22:32 2017
                 Z(phy_size)=2919 V(v_size) = 2919 stat_size=2919
                 M(mailbox_guid)=ad54230e65b49a59381100009c60b9f7
                 G(mail_guid)=a3d69f2868b49a596a1d00009c60b9f7
                 I(rbox_version): 0.1
[..]
```

NOTE: alternative - "rados -p rados_mail --all ls"; "for i in `rados -p rados_mail -N $N ls`; do rados -p rados_mail -N $N stat $i >> stats; done ; sort -k3,3 -k4,4 -n stats ; rm stats" ; "rados -p rados_mail get -N $N $Object_id test; cat test" ; "for i in `rados -p rados_mail listxattr -N $N $Object_id`; do echo -n $i: ; rados -p rados_mail getxattr -N $N $Object_id $i; echo "" ; done"


 <!-- .slide: data-state="normal" id="librmb-DT-2.31" data-timing="20s" data-menu-title="CephFS structure" -->
 ## CephFS structure

```bash
 ./06/0ad/20[..]589
 ./06/0ad/20[..]589/metadata.sqlite
 ./06/0ad/20[..]589/quota.dict
 ./06/0ad/20[..]589/dovecot-acl-list
 ./06/0ad/20[..]589/dovecot-uidvalidity.5cc6da8b
 ./06/0ad/20[..]589/dovecot-uidvalidity
 ./06/0ad/20[..]589/mailboxes
 ./06/0ad/20[..]589/mailboxes/INBOX
 ./06/0ad/20[..]589/mailboxes/INBOX/rbox-Mails
 ./06/0ad/20[..]589/mailboxes/INBOX/rbox-Mails/dovecot.index.log.2
 ./06/0ad/20[..]589/mailboxes/INBOX/rbox-Mails/dovecot.index.cache
 ./06/0ad/20[..]589/mailboxes/INBOX/rbox-Mails/dovecot.index.search.uids
 ./06/0ad/20[..]589/mailboxes/INBOX/rbox-Mails/dovecot.index.log
 ./06/0ad/20[..]589/mailboxes/INBOX/rbox-Mails/dovecot.index
 ./06/0ad/20[..]589/mailboxes/INBOX/rbox-Mails/dovecot.index.search
 ./06/0ad/20[..]589/mailboxes/INBOX/rbox-Mails/dovecot.index.backup
```


<!-- .slide: data-state="normal" id="librmb-DT-2.4" data-timing="20s" data-menu-title="rados-dict" -->
## RADOS Dictionary Plugin

### make use of Ceph omap key/value store
### RADOS namespaces
 * `shared/<key>`
 * `priv/<key>`

### to store metadata, quota, ...


<!-- .slide: data-state="normal" id="librmb-DT-3" data-timing="20s" data-menu-title="librmb" -->
## It's open source!

<div>
    <img style="position: absolute; width: 65%; left: 45%;" alt="Github Project Screenshot"
         data-src="images/github-ceph-dovecot_new.png" />
</div> <!-- .element: class="fragment" data-fragment-index="2" -->

### <span>License: `LGPLv2.1`</span><!-- .element: class="fragment" data-fragment-index="0" -->

### <span>Language: `C++`</span> <!-- .element: class="fragment" data-fragment-index="1" -->

<span class="fragment" data-fragment-index="2">
### Supported Dovecot versions: <!-- .element: class="fragment" data-fragment-index="2" -->
* 2.2 >= 2.2.21 <!-- .element: class="fragment" data-fragment-index="2" -->
* 2.3 <!-- .element: class="fragment" data-fragment-index="2" -->
</span>

### <span><a href="https://github.com/ceph-dovecot/">github.com/ceph-dovecot/</a></span> <!-- .element: class="fragment" data-fragment-index="3" -->

### still under development <!-- .element: class="fragment" data-fragment-index="4" -->


<!-- .slide: data-state="normal" id="ceph-version" data-timing="20s" data-menu-title="Ceph version" -->
## Which minimum Ceph Release?

<div>
     <img style="width: 60%; left: 47%; position: absolute" alt="ceph luminous"
          data-src="images/luminous_logo.png" />
</div> <!-- .element: class="fragment" data-fragment-index="5" -->

### Required Features: <!-- .element: class="fragment" data-fragment-index="0" -->

* <!-- .element: class="fragment" data-fragment-index="1" --> Bluestore
  * <!-- .element: class="fragment" data-fragment-index="1" --> write performance is critical
* <!-- .element: class="fragment" data-fragment-index="2" --> CephFS
  * <!-- .element: class="fragment" data-fragment-index="2" --> Multi-MDS
* <!-- .element: class="fragment" data-fragment-index="3" --> Erasure coding

### Enterprise currently products used: <!-- .element: class="fragment" data-fragment-index="6" -->
* <!-- .element: class="fragment" data-fragment-index="6" --> SES 5.5, SLES 12-SP3
* <!-- .element: class="fragment" data-fragment-index="7" --> Target: SES 6, SLES 15

Note:
- Target for PoC is SES 6 based on Nautilus
