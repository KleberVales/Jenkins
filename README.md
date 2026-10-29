# Block Storage ‐ advanced

# 1. Block volume

## Resizing a Volume

Resizing a block volume in OCI allows you to increase storage capacity, but you cannot decrease it. 

Online resizing is the preferred method, allowing you to expand a volume without detaching it from an instance. 

If cross-region replication is enabled, you must disable it before resizing and re-enable it afterward. 

After resizing, you must extend the partition, and if the volume is attached, you must also run rescan commands. 

Offline options include expanding a detached volume, restoring from a backup to a larger volume, or cloning to a new, larger volume. Remember that the first backup after a resize is a full backup, and the key rule is that volume size can only increase.

---

**Volume Size Can Only Increase**

The most important rule to remember:

- You can only increase the size of a volume.
- You cannot decrease the size.

Example: If a volume is 1,024 GB, you can increase it to up to 32 TB, but you cannot decrease it to 512 GB.

---

**Online Resizing**

What Is Online Resizing?

Online resizing allows you to expand an existing volume in place without detaching the volume from an instance.

Key Characteristics
| Feature | Description |
|---|---|
| Detachment Required | No |
| Size Change | Increase only |
| Recommended | Yes, preferred way to avoid impact on applications or workloads |
| First Backup After Resize | Full backup |

Cross-Region Replication Consideration

If cross-region replication is enabled for the volume you want to resize:

1. Disable cross-region replication before resizing.
2. Resize the volume.
3. Re-enable cross-region replication after resizing.

Post-Resize Steps

After the resize operation:

- You must extend the partition for the resize to take effect.
- If the volume is attached to an instance, you must also run the applicable rescan commands in addition to extending the partition manually.
- If the volume is not attached to any instance, you only need to extend the partition.

---

**Offline Resizing Options**

There are three offline options to increase volume size:

1. Expand an Existing Detached Volume
- Detach the volume from the instance before expanding.
- Resize the volume.
- Reattach the volume.
- Extend the partition.
- No rescan needed because the volume was detached.

2. Restore from a Volume Backup to a Larger Volume
- While restoring a backup, you can also increase the size of the volume.

3. Clone an Existing Volume to a New, Larger Volume
- Clone an existing volume to a new, larger volume.
- Backups and clones are covered in upcoming lessons.

---


