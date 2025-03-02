---
# ----------------------------------------------------------------------------
#
#     ***     AUTO GENERATED CODE    ***    Type: MMv1     ***
#
# ----------------------------------------------------------------------------
#
#     This file is automatically generated by Magic Modules and manual
#     changes will be clobbered when the file is regenerated.
#
#     Please read more about how to change this file in
#     .github/CONTRIBUTING.md.
#
# ----------------------------------------------------------------------------
subcategory: "Compute Engine"
layout: "google"
page_title: "Google: google_compute_node_group"
sidebar_current: "docs-google-compute-node-group"
description: |-
  Represents a NodeGroup resource to manage a group of sole-tenant nodes.
---

# google\_compute\_node\_group

Represents a NodeGroup resource to manage a group of sole-tenant nodes.


To get more information about NodeGroup, see:

* [API documentation](https://cloud.google.com/compute/docs/reference/rest/v1/nodeGroups)
* How-to Guides
    * [Sole-Tenant Nodes](https://cloud.google.com/compute/docs/nodes/)

~> **Warning:** Due to limitations of the API, Terraform cannot update the
number of nodes in a node group and changes to node group size either
through Terraform config or through external changes will cause
Terraform to delete and recreate the node group.

<div class = "oics-button" style="float: right; margin: 0 0 -15px">
  <a href="https://console.cloud.google.com/cloudshell/open?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fterraform-google-modules%2Fdocs-examples.git&cloudshell_working_dir=node_group_basic&cloudshell_image=gcr.io%2Fgraphite-cloud-shell-images%2Fterraform%3Alatest&open_in_editor=main.tf&cloudshell_print=.%2Fmotd&cloudshell_tutorial=.%2Ftutorial.md" target="_blank">
    <img alt="Open in Cloud Shell" src="//gstatic.com/cloudssh/images/open-btn.svg" style="max-height: 44px; margin: 32px auto; max-width: 100%;">
  </a>
</div>
## Example Usage - Node Group Basic


```hcl
resource "google_compute_node_template" "soletenant-tmpl" {
  name      = "soletenant-tmpl"
  region    = "us-central1"
  node_type = "n1-node-96-624"
}

resource "google_compute_node_group" "nodes" {
  name        = "soletenant-group"
  zone        = "us-central1-f"
  description = "example google_compute_node_group for Terraform Google Provider"

  size          = 1
  node_template = google_compute_node_template.soletenant-tmpl.id
}
```
<div class = "oics-button" style="float: right; margin: 0 0 -15px">
  <a href="https://console.cloud.google.com/cloudshell/open?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fterraform-google-modules%2Fdocs-examples.git&cloudshell_working_dir=node_group_autoscaling_policy&cloudshell_image=gcr.io%2Fgraphite-cloud-shell-images%2Fterraform%3Alatest&open_in_editor=main.tf&cloudshell_print=.%2Fmotd&cloudshell_tutorial=.%2Ftutorial.md" target="_blank">
    <img alt="Open in Cloud Shell" src="//gstatic.com/cloudssh/images/open-btn.svg" style="max-height: 44px; margin: 32px auto; max-width: 100%;">
  </a>
</div>
## Example Usage - Node Group Autoscaling Policy


```hcl
resource "google_compute_node_template" "soletenant-tmpl" {
  name      = "soletenant-tmpl"
  region    = "us-central1"
  node_type = "n1-node-96-624"
}

resource "google_compute_node_group" "nodes" {
  name        = "soletenant-group"
  zone        = "us-central1-f"
  description = "example google_compute_node_group for Terraform Google Provider"
  maintenance_policy = "RESTART_IN_PLACE"
  maintenance_window {
    start_time = "08:00"
  }
  initial_size  = 1
  node_template = google_compute_node_template.soletenant-tmpl.id
  autoscaling_policy {
    mode      = "ONLY_SCALE_OUT"
    min_nodes = 1
    max_nodes = 10
  }
}
```

## Argument Reference

The following arguments are supported:


* `node_template` -
  (Required)
  The URL of the node template to which this node group belongs.


- - -


* `description` -
  (Optional)
  An optional textual description of the resource.

* `name` -
  (Optional)
  Name of the resource.

* `size` -
  (Optional)
  The total number of nodes in the node group. One of `initial_size` or `size` must be specified.

* `initial_size` -
  (Optional)
  The initial number of nodes in the node group. One of `initial_size` or `size` must be specified.

* `maintenance_policy` -
  (Optional)
  Specifies how to handle instances when a node in the group undergoes maintenance. Set to one of: DEFAULT, RESTART_IN_PLACE, or MIGRATE_WITHIN_NODE_GROUP. The default value is DEFAULT.

* `maintenance_window` -
  (Optional)
  contains properties for the timeframe of maintenance
  Structure is [documented below](#nested_maintenance_window).

* `autoscaling_policy` -
  (Optional)
  If you use sole-tenant nodes for your workloads, you can use the node
  group autoscaler to automatically manage the sizes of your node groups.
  Structure is [documented below](#nested_autoscaling_policy).

* `zone` -
  (Optional)
  Zone where this node group is located

* `project` - (Optional) The ID of the project in which the resource belongs.
    If it is not provided, the provider project is used.


<a name="nested_maintenance_window"></a>The `maintenance_window` block supports:

* `start_time` -
  (Required)
  instances.start time of the window. This must be in UTC format that resolves to one of 00:00, 04:00, 08:00, 12:00, 16:00, or 20:00. For example, both 13:00-5 and 08:00 are valid.

<a name="nested_autoscaling_policy"></a>The `autoscaling_policy` block supports:

* `mode` -
  (Required)
  The autoscaling mode. Set to one of the following:
    - OFF: Disables the autoscaler.
    - ON: Enables scaling in and scaling out.
    - ONLY_SCALE_OUT: Enables only scaling out.
    You must use this mode if your node groups are configured to
    restart their hosted VMs on minimal servers.
  Possible values are `OFF`, `ON`, and `ONLY_SCALE_OUT`.

* `min_nodes` -
  (Optional)
  Minimum size of the node group. Must be less
  than or equal to max-nodes. The default value is 0.

* `max_nodes` -
  (Required)
  Maximum size of the node group. Set to a value less than or equal
  to 100 and greater than or equal to min-nodes.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `id` - an identifier for the resource with format `projects/{{project}}/zones/{{zone}}/nodeGroups/{{name}}`

* `creation_timestamp` -
  Creation timestamp in RFC3339 text format.
* `self_link` - The URI of the created resource.


## Timeouts

This resource provides the following
[Timeouts](/docs/configuration/resources.html#timeouts) configuration options:

- `create` - Default is 20 minutes.
- `update` - Default is 20 minutes.
- `delete` - Default is 20 minutes.

## Import


NodeGroup can be imported using any of these accepted formats:

```
$ terraform import google_compute_node_group.default projects/{{project}}/zones/{{zone}}/nodeGroups/{{name}}
$ terraform import google_compute_node_group.default {{project}}/{{zone}}/{{name}}
$ terraform import google_compute_node_group.default {{zone}}/{{name}}
$ terraform import google_compute_node_group.default {{name}}
```

## User Project Overrides

This resource supports [User Project Overrides](https://www.terraform.io/docs/providers/google/guides/provider_reference.html#user_project_override).
