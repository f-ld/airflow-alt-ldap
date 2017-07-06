An alternative LDAP backend for airflow
=======================================

The default LDAP backend works with OpenLDAP if the memberOf overlay is
activated (see http://www.openldap.org/doc/admin24/overlays.html#Reverse%20Group%20Membership%20Maintenance)
I.e., users must present the `memberOf` attribute to know what group they
belong to. If your LDAP server only has groups with `memberUid` (or any
other key like `member`) listing the users belonging to the group, then
you need something different. This is what this module attemps to provide.

Installation
============

Using pip:

```
pip install airflow-alt-ldap
```

Configuration
=============

Activate authentication via this LDAP backend in `airflow.cfg` config:

```
[webserver]
authenticate = True
auth_backend = airflow-alt-ldap.auth.backend.ldap_auth
```

Then you can configure that module using the following keys (example conf to be adapted):
```
uri = ldap://localhost:389
user_basedn = ou=people,dc=nexmo,dc=com
user_filter = uid=*
user_name_attr = uid
group_basedn = ou=groups,dc=nexmo,dc=com
group_member_attr = memberUid
group_filter = cn=*
superuser_filter = cn=admingroup
data_profiler_filter = cn=datagroup
bind_user = uid=binddn,dc=example,dc=com
bind_password = MyAwesomePassword
# cacert = /etc/ca/ldap_ca.crt
# Set search_scope to one of them:  BASE, LEVEL , SUBTREE
# Set search_scope to SUBTREE if using Active Directory, and not specifying an Organizational Unit
search_scope = SUBTREE

```

