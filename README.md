# puppet-gluster

This is my puppet-gluster module.  I use it to model and monitor my glusterfs 
bricks, servers, and clients -- but not pools or volumes.

## Usage:

### Overview:

1. Set up servers using puppet
1. Format & mount brick filesystems
1. Add brick declarations to server host manifests (see example below)
1. Probe servers appropriately using gluster cli
1. Create volumes as needed
1. Add client declarations to client host manifests (see example below)

### A server with a brick /dev/sde1:

    include gluster::server
    gluster::server::brick {
        "/bricks/myvol0": device => "e1";
    }

### A client mounting myvol_staging from server1:

    include gluster
    gluster::client::volume {
        "myvol_staging":
            mount_point => "/mnt/myvol",
            volfile_server => "server1.my.domain.net",
            mounted => "mounted";
    }

### Targeted self-heal

This module also provides the reference implementation of [targeted self-heal].

Requirements to use this feature are:

1. Glusterfs version in 3.1 or 3.2 series.  It's not necessary on Glusterfs 3.3+
1. Bricks are named <volume>N in a common brick parent directory (eg. /bricks/myvol0)
1. The server with bricks in the volume also has a client mount of the volume

To use, add a gluster::selfheal::hourly declaration to each server with bricks 
in the volume...

    gluster::selfheal::hourly {
        "myvol_staging":
            mount_point => "/mnt/myvol",
            mmin => "-1440";
    }

I _highly_ recommend reviewing & customizing this feature to suit your own needs.  

## Dependencies:

- My nagios module (yet to be released; replace with your own)
- My logstash module (yet to be released; comment or remove, for now)

## License:

- MIT License (see COPYING file)

[targeted self-heal]: http://community.gluster.org/a/howto-targeted-self-heal-repairing-less-than-the-whole-volume/