# docker-rpm-buildhost-el7
## Centos 7 RPM Build Host

This is an rpm building environment for RHEL 7 and its derivatives (CentOS 7, Oracle Linux 7, etc).  It is meant to be run as an interactive container rather than as a daemon.  In theory, it should provide a building environment for RHEL 7 on non-RHEL-7 platforms, or provide for an easy clean-up once finished building RPMS - allowing you to keep your server clean of devel packages and the like.

You may want to edit the ~builder/.rpmmacros file to suit your details.  Below, I map my personal copy of it into the container.  Without, you can get a generic one there and copy it out to make your own personalization. (packager, distribution, and vendor tags, specifically)

Put your sources in ~builder/rpms/SOURCES, specs in ~builder/rpms/SPECS, and away you go.  It's a good idea to mount ~builder/rpms on an external volume, since these containers are temporary use and you might want to keep these around.

"sudo su -" is set up to add additional packages as needed when building RPMs, and "wget" is included for getting updated sources without exiting the container.

Finally, http://www.rpm.org/max-rpm-snapshot/ is a great reference for learning about building RPMs with rpm-build.

I typically run this container with command similar to:

```
docker run --rm -it \
        --hostname=buildhost_el7 \
        -v /usr/local/src/rpms/SPECS:/home/builder/rpms/SPECS \
        -v /usr/local/src/rpms/SOURCES:/home/builder/rpms/SOURCES \
        -v /usr/local/src/rpms/RPMS:/home/builder/rpms/RPMS \
        -v /usr/local/src/rpms/SRPMS:/home/builder/rpms/SRPMS \
        -v /usr/local/src/rpms/rpmmacros:/home/builder/.rpmmacros \
        guyton/rpm-buildhost-el7
```

Once up, you should start in your SPECS dir, and away you go.

## Things to do

- Get set up for gpg signing
- Better linking with yumrepo container
- Add git if might start needing for source fetching


