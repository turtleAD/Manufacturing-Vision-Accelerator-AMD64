# Manufacturing Vision Accelerator for AMD64 architectures
Welcome to the Manufacuring Vision Accelerator repo - we hope what you find here may be helpful to your pursuit of vision analytics on the Edge! The main portion of this code and process flow was created by myself and Nick Kwiecien, Ph.D., but there are a great number of tehnical collaborators as well on this AIoT journey, including Rick Durham (Azure AI/ML) and Keith Hill, Azure PM.  

This is a sample repository of working code - nothing more, nothing less.  There are no warranties or guarantees stated, or otherwise implied.


[Architecture Walkthrough Video](/video/architectural_overview.mp4)


[Demo Walkthrough Video](/video/Demo_Walkthrough.mp4)


[Solution Accelerator Overview Video](/video/Solution_Accelerator_Overview.mp4)


[Azure Resource Setup Video](/video/Azure_Setup_Walkthrough.mp4)


[Click here to access the Hands-on-Lab](/Hands-on-Lab/Hands-on-Lab.md)



# Module Deep Dives

[Part 1 - CIS (capture|inference|store) module](/video/cis_camera_module_part_1.mp4)

[Part 2 - CIS (capture|inference|store) module](/video/cis_camera_module_part_2.mp4)

[Module Twin Configuration Tool module](/video/module_twin_configuration.mp4)

[Model Repo module](/video/model_repo.mp4)

[Custom Dashboard module](/video/custom_dashboard.mp4)

[Image Upload module](/video/image_upload.mp4)

[File Cleanup module](/video/file_cleanup.mp4)

[File Upload/Download Utiliy modules](/video/file_upload_download.mp4)

As you get started on your journey with the Solution Accelerator, here are a few application notes to keep in mind:  
    
    1) Once you've deployed IoT Edge, you will need to either 'vi' or 'nano' into the /etc/docker/daemon.json - if this file doesn't exist, go ahead and create it.  in here, I recommend adding some implicit values that apply to all containers.  
    
    For CPU-only deployments, I would use something simlar to the following:

    {
        "dns": ["8.8.8.8"],
        "log-driver": "json-file",
        "log-opts": {
            "max-size": "10m",
            "max-file": "3"
        }
    }

    For Nvidia GPU-based deployments, I would paste the following:

    {
        "dns": ["8.8.8.8"],
        "default-runtime": "nvidia",
        "runtimes": {
            "nvidia": {
                "path": "nvidia-container-runtime",
                "runtimeArgs": []
            }
        },
        "log-driver": "json-file",
        "log-opts": {
            "max-size": "10m",
            "max-file": "3"
        }
    }

    2) Once you've built and deployed the solution containers, a new directory at /home/edge_assets will be created on the Edge device.  If you want to use the sample models and images contained in this repository for testing, you will need to elevate the permissions of this directory by running:

    sudo chmod -R 777 /home/edge_assets


# About our 'vision' 

This project was built out of necessity - the need to provide for that 'last mile' connectivity of remote vision workloads while also solving for the unique challenges of these use cases.  Taking a 'code-first' approach across a number of deep engagements with multiple manufacturing customers, we began to see universal patterns emerge:

1. Build for flexibility: 
 
 Taking a 'code-first' approach meant being deliberate about choices in terms of programming language and the packages used.  For this project, we coded everything in Python, and used only open source elements, such as OpenCV for image capture/manipulation and the Open Neural Network Exchange (ONNX) format for vision models. The goal is to provide a usable code base as is, but malleable enough that customers and/or our partners can easily customize it as well.

2. Address the 80%:  

 Within our vision engagements, we found that 80%+ of the use cases revolved around anomaly detection, so the focus of this repository is on object detection.  This could be detecting the presence of foreign objects in a production line, detection of errors in assembly, safety infractions such as the lack of PPE or a spill/leakage of product.  While image classification and segmentation are also important areas which this repository will expand into eventually, the initial focus is on the majority use case.   We will also, in time, provide patterns for integrating Computer Vision services such as OCR to augment capabilities.

3. Plan for model retraining:

 What many of the out-of-the-box solutions fail to plan for the eventual need for model retraining.  A vision solution has to have the ability to augement its datasets, as model drift or 'data' drift happens - it's not a question of if, but a question of when.  On the model side, perhaps there is a bias in your classes that gets revealed during production inferencing which requires a rebalancing of the class representation.  On the 'data' side, it could be that a camera gets knocked out of aligment, the lighting in a plant changes or seasonal changes change the available natural light, etc.

4. Always think about the Enterprise perspective: 

 The other challenge we've seen consistently in pre-built solutions is the propensity to create yet another silo.  Although the lower layers of the ISA-95 network have traditionally been partially or totally 'airgapped' in Manufacturing, the current trend towards Industry 4.0 has created a paradigm shift to connect OT networks to IT systems.  Vision systems have also traditionally been in that 'air-gapped' mode, but few have made the transition to Enterprise transparency.  From what we've observed, vision analytics systems provide intrinsic value to the front-line plant engineers and workers, but that unique visibility into operations across an Enterprise has value all the way to the C-suite.  From the QA to ERP to MES, vision analytics change the way organizations do business for the better.
