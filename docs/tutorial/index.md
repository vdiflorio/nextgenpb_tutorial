---
title: Tutorial
nav_order: 2
---

# Tutorial
{: .fs-9 }

Here are a few practical examples to become familiar with **NextGenPB**.
{: .fs-6 .fw-300 }

---

## Disclaimer

In this tutorial, we will use the generic precompiled **Apptainer container**.  
If you need a custom-built version (e.g., optimized for specific hardware or tuned for performance), please refer to the [container installation guide](/nextgenpb_tutorial/docs/guide/installation/container).

If you are a developer (or simply prefer installing the code directly on your system), installation instructions are available for both [macOS](/nextgenpb_tutorial/docs/guide/installation/mac) and [Linux](/nextgenpb_tutorial/docs/guide/installation/linux).

{: .important }
> If you want to learn more about the input parameters and run options in detail,  
> please refer to the [Guide](/nextgenpb_tutorial/docs/guide).

---

## Minimum Requirements

| Requirement       | Description                              |
|-------------------|------------------------------------------|
| OS                | Linux      |
| Apptainer version | ≥ 1.1                                    |
| CPU               | x86_64 architecture                      |


{: .note-title }
> Note for macOS users
>
> Apptainer is not supported on macOS. You can build and use the Docker container instead by [following these instructions](/nextgenpb_tutorial/docs/guide/installation/container), and adapt the commands in this tutorial to use `docker exec` instead of `apptainer exec`. For more details, see the [running containers guide](/nextgenpb_tutorial/docs/guide/run/container).
