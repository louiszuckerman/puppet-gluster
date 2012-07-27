# puppet-gluster

This is my puppet-gluster module.  I use it to manage my gluster bricks, 
servers, and clients -- but not pools or volumes.

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

## Dependencies:

- My nagios module (yet to be released)
- My logstash module (yet to be released)
