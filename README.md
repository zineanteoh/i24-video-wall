# i24videowall

Documentation for all setups related to the i24 video wall.

# Table of Contents

1. [Setting up the 9 monitors to display as one desktop](#monitors)
2. [Setting up Elasticsearch-Logstash-Kibana (ELK) for visualization](#elk)
3. [Setting up Camera Streaming on VLC with RTSP](#camera)

---

## Setting up the 9 monitors to display as one desktop <a name="monitors"></a>

> References required for this step: [NVIDIA Quadro Sync II Manual](https://images.nvidia.com/content/quadro/product-literature/user-guides/Quadro-Sync-II-User-Guide.pdf) and [NVIDIA Mosaic Manual](https://images.nvidia.com/aem-dam/en-zz/Solutions/design-visualization/quadro-product-literature/NVMosaic-UG.pdf)

1.  **Install Windows**
2.  **Install NVIDIA RTX Enterprise Driver** (https://nvidia.com/drivers)

    ![Nvidia Driver](/assets/nvidia.png)

3.  **Connect Monitors:**

    If all 9 monitors are not displaying windows screen, then go to `NVIDIA Control Panel` > `3D Settings` > `Set SLI and PhysX Configuration` > `SLI Configuration (Activate all displays)` > `Apply`

4.  **Setup Mosaic:**

    Create a single desktop from multiple displays and GPUs by going to NVIDIA Control Panel > Set up Mosaic

    | Select Toplogy     |           |
    | ------------------ | --------- |
    | Number of displays | 9         |
    | Topology           | 3x3       |
    | Orientation        | landscape |
    | GPU Topology       | maximum   |

    | Select Displays        |             |
    | ---------------------- | ----------- |
    | Displays               | check all   |
    | Refresh Rate           | 60Hz        |
    | Resolution per display | 1920 x 1080 |

    | Arrange Displays                                  |
    | ------------------------------------------------- |
    | Drag and drop displays until the topology matches |

    | Setup bezel correction |
    | ---------------------- |
    | If necessary           |

---

## Setting up Elasticsearch-Logstash-Kibana (ELK) for visualization <a name="elk"></a>

> References required for this step: [Docker-ELK Github Readme](https://github.com/deviantony/docker-elk)

![Visualization for Elasticsearch Logstash Kibana](/assets/elk.png)

1.

---

## Setting up Camera Streaming on VLC with RTSP <a name="camera"></a>

> References required for this step: [RTSP Command](https://learncctv.com/rtsp-commands-for-axis-cameras/)

1. Install VLC on Windows
2. Go to `File > Open Network Stream option`
3. Enter the command `rtsp://<username>:<password>@<camera-ip-address>/axis-media/media.amp`
