# i24videowall

Documentation for all setups related to the i24 video wall.

# Table of Contents

## Hardware setup

1. [Setting up NVIDIA X server](#monitors)

## Visualization setup

1. [Setting up Elasticsearch-Logstash-Kibana (ELK)](#elk)
2. [Setting up Camera Streaming](#camera)
3. [Running Elasticsearch–Logstash-Kibana (ELK)](#runelk)

---

## Setting up NVIDIA X server <a name="monitors"></a>

> This documents the steps required to set up the 9 monitors to display as one desktop (also known as an X server).

> References required for this step: [NVIDIA Quadro Sync II Manual](https://images.nvidia.com/content/quadro/product-literature/user-guides/Quadro-Sync-II-User-Guide.pdf) and [NVIDIA Mosaic Manual](https://images.nvidia.com/aem-dam/en-zz/Solutions/design-visualization/quadro-product-literature/NVMosaic-UG.pdf)

1.  **Install Windows**
2.  **Install NVIDIA RTX Enterprise Driver** (https://nvidia.com/drivers)

    ![Nvidia Driver](/assets/nvidia.png)

3.  **Connect Monitors:**

    If all 9 monitors are not displaying windows screen, then proceed to
    `NVIDIA Control Panel` > `3D Settings` > `Set SLI and PhysX Configuration` > `SLI Configuration (Activate all displays)` > `Apply`

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

## Setting up Elasticsearch-Logstash-Kibana (ELK) <a name="elk"></a>

> This documents the steps required to set up the ELK pipeline using Docker. After configuring the elk stack, running the docker instance will start logstash, which processes the logs and store them in elasticsearch. Finally, Kibana is used to visualize the data as a dashboard.

> References required for this step: [Docker-ELK Github Readme](https://github.com/deviantony/docker-elk)

![Visualization for Elasticsearch Logstash Kibana](/assets/elk.png)

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) for Windows
2. Create a python virtual env

    `python3 -v venv venv`

3. Activate the python environment

    `.\venv\Script\activate`

4. Install dependencies

    `pip install python-logstash-async`

5. Clone [docker-elk](https://github.com/deviantony/docker-elk), which allows us to run the Elastic stack on Docker.
6. Configure `\docker-elk\logstash\pipeline\logstash.conf` file to process json, filter out hosts, and specify its input source and output destination:

    ```
    input {
        tcp {
            port => 5000
            codec => json
        }
    }

    ## Add your filters / logstash plugins configuration here

    output {
        elasticsearch {
            hosts => "elasticsearch:9200"
            user => "logstash_internal"
            password => "${LOGSTASH_INTERNAL_PASSWORD}"
        }
    }
    filter {
        mutate {
            remove_field => [ "host" ]
        }
    }
    ```

7. Enter `docker-compose up` in commmand prompt to run the Docker instance
8. Visit `localhost:5601` to open Kibana.

---

## Setting up Camera Streaming <a name="camera"></a>

> This documents the steps required to stream the camera feed on VLC app with RTSP (Real-Time Streaming Protocol). It is necessary to have the ip address of the camera and a user account username and password to access the stream.

> References required for this step: [RTSP Command](https://learncctv.com/rtsp-commands-for-axis-cameras/)

1. Install VLC on Windows
2. Go to `File > Open Network Stream option`
3. Enter the command `rtsp://<username>:<password>@<camera-ip-address>/axis-media/media.amp`

---

## Running Elasticsearch–Logstash-Kibana (ELK) <a name="runelk"></a>

> Step-by-step on how to run ELK instance. This assumes that ELK has [already been set up.](#elk)

1. Open command prompt and `cd` into `elk-stack` folder.
2. Activate the virtual environment `.\venv\Scripts\activate`
3. `cd` into `docker-elk` folder
4. Type `docker-compose up` to start the docker instance
5. Visit `localhost:5601` to open Kibana.
6. Enter username and password to login.
