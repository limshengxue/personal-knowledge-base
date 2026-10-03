2026-02-28 12:17

# 14 - Volume
- All data store in container get deleted when container is deleted
- Volume is a storage abstractions used by k8s that allow various storage technologies to be used
- It is a mount point of the container file system backed by storage medium
- The storage medium, content, and access mode are determined by the Volume type
- Volume can be shared among containers


![[Attachments/Pasted image 20260228122039.png]]


## Container Storage Interface (CSI)
- CSI is a standardize interface to allow different storage vendors to work with different orchestrators which include K8s

## Volume Types
A directory which is mounted inside a Pod is backed by the underlying Volume Type
Which decide the size, content, access mode, etc
- emptyDir - life tie with the Pod, linked to Pod
- hostPath - shared with host 
- gce, azure, aws disk
- etc

## Persistent Volume
 - Provide APIs for users and admin to manage and consume persist storage
 - PersistVolume - manage
 - PersistVolumeClaim - consume
 - Provision by cluster admin, can be dynamically provisioned based on the StorageClass resource
 - StorageClass define provisioners and parameters to create PersistentVolume

## PersistentVolumeClaims (PVC)
- A request for storage by user
- Based on storage class, access mode, size
- Access Mode
	- ReadWriteOnce (by single node)
	- ReadWriteMany (by many node)
	- ReadOnlyMany
	- ReadWriteOncePod (read-write by a single pod)
- When the PV is released, the PV can be reclaimed, deleted (data and volume delete), or recycled (only delete the data)

![[Attachments/Pasted image 20260228133133.png]]


An example use of Volume with a Deployment resources
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  # Name of this Deployment resource
  name: blue-app
  labels:
    # Label on the Deployment object itself (not the pods)
    app: blue-app

spec:
  # How many pod replicas to run
  replicas: 1

  # Deployment manages pods whose labels match this selector
  selector:
    matchLabels:
      app: blue-app

  template:
    metadata:
      # Labels applied to the created Pod (these are important for Service selectors)
      labels:
        app: blue-app
        type: canary

    spec:
      # -----------------------------
      # VOLUMES (Pod-level)
      # -----------------------------
      # "volumes" defines storage sources that ALL containers in this Pod can mount.
      volumes:
        - name: host-volume
          # hostPath means: use a DIRECTORY on the Kubernetes NODE's filesystem.
          # So the data lives on the host machine running this pod (not in the container).
          hostPath:
            # This path must exist on the node, or Kubernetes may create it depending on type (not set here).
            path: /home/docker/blue-shared-volume

      containers:
        # -----------------------------
        # Container 1: nginx web server
        # -----------------------------
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80

          # volumeMounts (container-level) says WHERE to mount the pod volume inside this container.
          volumeMounts:
            - name: host-volume          # refers to volumes[].name above
              mountPath: /usr/share/nginx/html
              # nginx serves static files from /usr/share/nginx/html by default
              # so whatever is in the hostPath directory becomes the website content

        # -----------------------------
        # Container 2: debian "writer"
        # -----------------------------
        - name: debian
          image: debian

          # Mount the SAME pod volume into this container at a different path
          volumeMounts:
            - name: host-volume
              mountPath: /host-vol

          # This container writes a file into the shared volume, then stays alive.
          # Result: it creates/overwrites /host-vol/index.html (which is the hostPath dir)
          # and nginx immediately serves that file because it mounts the same volume.
          command:
            - /bin/sh
            - -c
            - |
              echo Welcome to BLUE App! > /host-vol/index.html
              sleep infinity
```

# References
