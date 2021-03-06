---
title: Configuration
category: Getting Started
chapter: 1
order: 5
---

The central configuration file `application.properties` resides in the classpath of the WAR by default. 
This configuration file controls many performance tuning parameters but is most useful for defining
optional external database sources, directory services (LDAP), and proxy settings.

Dependency-Track administrators are highly encouraged to create a copy of this file in the
Dependency-Track data directory and customize it prior to deploying to production.


> The default embedded H2 database is designed to quickly evaluate and experiment with Dependency-Track.
> Do not use the embedded H2 database in production environments. 
> 
> See: [Database Support]({{ site.baseurl }}{% link _docs/getting-started/database-support.md %}).


To start Dependency-Track using custom configuration, add the system property 
`alpine.application.properties` when executing. For example:

```bash
-Dalpine.application.properties=~/.dependency-track/application.properties
```

#### Default configuration

```ini
############################ Alpine Configuration ###########################

# Required
# Defines the number of worker threads that the event subsystem will consume.
# Events occur asynchronously and are processed by the Event subsystem. This
# value should be large enough to handle most production situations without
# introducing much delay, yet small enough not to pose additional load on an
# already resource-constrained server.
# A value of 0 will instruct Alpine to allocate 1 thread per CPU core. This
# can further be tweaked using the alpine.worker.thread.multiplier property.
# Default value is 0.
alpine.worker.threads=0

# Required
# Defines a multiplier that is used to calculate the number of threads used
# by the event subsystem. This property is only used when alpine.worker.threads
# is set to 0. A machine with 4 cores and a multiplier of 4, will use (at most)
# 16 worker threads. Default value is 4.
alpine.worker.thread.multiplier=4

# Required
# Defines the path to the data directory. This directory will hold logs, keys,
# and any database or index files along with application-specific files or 
# directories.
alpine.data.directory=~/.dependency-track

# Required
# Defines the interval (in seconds) to log general heath information. If value
# equals 0, watchdog logging will be disabled.
alpine.watchdog.logging.interval=0

# Required
# Defines the database mode of operation. Valid choices are:
# 'server', 'embedded', and 'external'.
# In server mode, the database will listen for connections from remote hosts.
# In embedded mode, the system will be more secure and slightly faster. 
# External mode should be used when utilizing an external database server 
# (i.e. mysql, postgresql, etc).
alpine.database.mode=embedded

# Optional
# Defines the TCP port to use when the database.mode is set to 'server'.
alpine.database.port=9092

# Required
# Specifies the JDBC URL to use when connecting to the database.
alpine.database.url=jdbc:h2:~/.dependency-track/db

# Required
# Specifies the JDBC driver class to use.
alpine.database.driver=org.h2.Driver

# Optional
# Specifies the path (including filename) to where the JDBC driver is located.
# alpine.database.driver.path=/path/to/dbdriver.jar

# Optional
# Specifies the username to use when authenticating to the database.
alpine.database.username=sa

# Optional
# Specifies the password to use when authenticating to the database.
# alpine.database.password=

# Optional
# Specifies if the database connection pool is enabled.
alpine.database.pool.enabled=true

# Optional
# This property controls the maximum size that the pool is allowed to reach,
# including both idle and in-use connections.
alpine.database.pool.max.size=10

# Optional
# This property controls the maximum amount of time that a connection is
# allowed to sit idle in the pool.
alpine.database.pool.idle.timeout=600000

# Optional
# This property controls the maximum lifetime of a connection in the pool.
# An in-use connection will never be retired, only when it is closed will
# it then be removed.
alpine.database.pool.max.lifetime=600000

# Optional
# When authentication is enforced, API keys are required for automation, and
# the user interface will prevent anonymous access by prompting for login
# credentials.
alpine.enforce.authentication=true

# Optional
# When authorization is enforced, team membership for both API keys and user
# accounts are restricted to what the team itself has access to. To enforce 
# authorization, the enforce.authentication property (above) must be true.
alpine.enforce.authorization=true

# Required
# Specifies the number of bcrypt rounds to use when hashing a users password.
# The higher the number the more secure the password, at the expense of
# hardware resources and additional time to generate the hash.
alpine.bcrypt.rounds=14

# Required
# Defines if LDAP will be used for user authentication. If enabled,
# alpine.ldap.* properties should be set accordingly.
alpine.ldap.enabled=false

# Optional
# Specifies the LDAP server URL
# Example (Microsoft Active Directory):
#    alpine.ldap.server.url=ldap://ldap.example.com:3268
#    alpine.ldap.server.url=ldaps://ldap.example.com:3269
# Example (ApacheDS, Fedora 389 Directory, NetIQ/Novell eDirectory, etc):
#    alpine.ldap.server.url=ldap://ldap.example.com:389
#    alpine.ldap.server.url=ldaps://ldap.example.com:636
alpine.ldap.server.url=ldap://ldap.example.com:389

# Optional
# Specifies the base DN that all queries should search from
alpine.ldap.basedn=dc=example,dc=com

# Optional
# Specifies the LDAP security authentication level to use. Its value is one of
# the following strings: "none", "simple", "strong". If this property is empty
# or unspecified, the behaviour is determined by the service provider.
alpine.ldap.security.auth=simple

# Optional
# If anonymous access is not permitted, specify a username with limited access
# to the directory, just enough to perform searches. This should be the fully
# qualified DN of the user.
alpine.ldap.bind.username=

# Optional
# If anonymous access is not permitted, specify a password for the username
# used to bind.
alpine.ldap.bind.password=

# Optional
# Specifies if the username entered during login needs to be formatted prior
# to asserting credentials against the directory. For Active Directory, the
# userPrincipal attribute typically ends with the domain, whereas the
# samAccountName attribute and other directory server implementations do not.
# The %s variable will be substitued with the username asserted during login.
# Example (Microsoft Active Directory):
#    alpine.ldap.auth.username.format=%s@example.com
# Example (ApacheDS, Fedora 389 Directory, NetIQ/Novell eDirectory, etc):
#    alpine.ldap.auth.username.format=%s
alpine.ldap.auth.username.format=%s@example.com

# Optional
# Specifies the Attribute that identifies a users ID
# Example (Microsoft Active Directory):
#    alpine.ldap.attribute.name=userPrincipalName
# Example (ApacheDS, Fedora 389 Directory, NetIQ/Novell eDirectory, etc):
#    alpine.ldap.attribute.name=uid
alpine.ldap.attribute.name=userPrincipalName

# Optional
# Specifies the LDAP attribute used to store a users email address
alpine.ldap.attribute.mail=mail

# Optional
# Specifies the LDAP search filter used to retrieve all groups from the
# directory.
# Example (Microsoft Active Directory):
#    alpine.ldap.groups.filter=(&(objectClass=group)(objectCategory=Group))
# Example (ApacheDS, Fedora 389 Directory, NetIQ/Novell eDirectory, etc):
#    alpine.ldap.groups.filter=(&(objectClass=groupOfUniqueNames))
alpine.ldap.groups.filter=(&(objectClass=group)(objectCategory=Group))

# Optional
# Specifies the LDAP search filter to use to query a user and retrieve a list
# of groups the user is a member of. The {USER_DN} variable will be substituted
# with the actual value of the users DN at runtime.
# Example (Microsoft Active Directory):
#    alpine.ldap.user.groups.filter=(&(objectClass=group)(objectCategory=Group)(member={USER_DN}))
# Example (Microsoft Active Directory - with nested group support):
#    alpine.ldap.user.groups.filter=(member:1.2.840.113556.1.4.1941:={USER_DN})
# Example (ApacheDS, Fedora 389 Directory, NetIQ/Novell eDirectory, etc):
#    alpine.ldap.user.groups.filter=(&(objectClass=groupOfUniqueNames)(uniqueMember={USER_DN}))
alpine.ldap.user.groups.filter=(member:1.2.840.113556.1.4.1941:={USER_DN})

# Optional
# Specifies if mapped LDAP accounts are automatically created upon successful
# authentication. When a user logs in with valid credentials but an account has
# not been previously provisioned, an authentication failure will be returned.
# This allows admins to control specifically which ldap users can access the
# system and which users cannot. When this value is set to true, a local ldap
# user will be created and mapped to the ldap account automatically. This
# automatic provisioning only affects authentication, not authorization.
alpine.ldap.user.provisioning=false

# Optional
# This option will ensure that team memberships for LDAP users are dynamic and
# synchronized with membership of LDAP groups. When a team is mapped to an LDAP
# group, all local LDAP users will automatically be assigned to the team if
# they are a member of the group the team is mapped to. If the user is later
# removed from the LDAP group, they will also be removed from the team. This
# option provides the ability to dynamically control user permissions via an
# external directory.
alpine.ldap.team.synchronization=false

# Optional
# HTTP proxy. If the address is set, then the port must be set too.
# alpine.http.proxy.address=proxy.example.com
# alpine.http.proxy.port=8888
# alpine.http.proxy.username=
# alpine.http.proxy.password=

# Optional
# Cross-Origin Resource Sharing (CORS) headers to include in REST responses.
# If 'alpine.cors.enabled' is true, CORS headers will be sent, if false, no
# CORS headers will be sent.
# See Also: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
# The following are default values
#alpine.cors.enabled=true
#alpine.cors.allow.origin=*
#alpine.cors.allow.methods=GET POST PUT DELETE OPTIONS
#alpine.cors.allow.headers=Origin, Content-Type, Authorization, X-Requested-With, Content-Length, Accept, Origin, X-Api-Key, X-Total-Count, *
#alpine.cors.expose.headers=Origin, Content-Type, Authorization, X-Requested-With, Content-Length, Accept, Origin, X-Api-Key, X-Total-Count
#alpine.cors.allow.credentials=true
#alpine.cors.max.age=3600

####################### Dependency-Track Configuration ######################

# Optional
# Specifies if VulnDB access is enabled or not. VulnDB is a commercial source
# of vulnerability data that requires a subscription. Enabling VulnDB provides
# vulnerability data that may not be published in public repositories and may
# enhance public vulnerability data with additional content.
# Refer to https://vulndb.cyberriskanalytics.com/ for information.
# datasource.vulndb.enabled=false
```

#### Proxy Configuration

Proxy support can be configured in one of two ways, using the proxy settings defined
in `application.properties` or through environment variables. By default, the system
will attempt to read the `https_proxy` and `http_proxy` environment variables. If one 
of these are set, Dependency-Track will use them automatically.

Dependency-Track supports proxies that require BASIC, DIGEST, and NTLM authentication.

#### Logging Levels

Logging levels (INFO, WARN, ERROR, DEBUG, TRACE) can be specified by passing the level 
to the `dependencyTrack.logging.level` system property on startup. For example, the 
following command will start Dependency-Track (embedded) with DEBUG logging:

```bash
java -Xmx4G -DdependencyTrack.logging.level=DEBUG -jar dependency-track-embedded.war
```

For Docker deployments, simply set the `LOGGING_LEVEL` environment variable to one of
INFO, WARN, ERROR, DEBUG, or TRACE.