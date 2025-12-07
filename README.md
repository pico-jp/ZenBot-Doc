# AMD_Robotics_Hackathon_2025_ZenBot

## Team Information

**Team:** ZenBot (Team 25)
**Members:** Eduardo Gonzalez, Masahiko Nakano

**Summary:**  
ZenBot is an autonomous robotic system that creates **Zen garden patterns**.  
Our system combines intuitive UI design, computer vision, and robot inference to allow anyone to create a traditional Japanese Zen garden through a robot.

![ZenBot places a rock.](./assets_for_doc/image_zenbot_arm.png)
![ZenBot's UI](./assets_for_doc/image_zenbot_ui.png)

---

## Submission Details

### 1. Mission Description
ZenBot aims to automate and augment the process of creating **Karesansui (Japanese Zen gardens)**, traditionally crafted by experienced gardeners.  
Our mission is to demonstrate how robotics + vision + user interfaces can help automate creative manual tasks such as:

- Drawing raked sand patterns  
- Planning spatial layouts  
- Reproducing design templates  
- Translating real-world garden images into executable robot instructions  

This system can be used for **education**, **cultural preservation**, **artistic prototyping**, and **low-cost automated landscaping demos**.

---

### 2. Creativity
- The project blends **ancient Japanese aesthetics** with modern robotics—an unusual and culturally rich mission within the hackathon.
- We built a **Zen Garden Planner UI**, combining UX minimalism with robotics control.  
  Users can manually build their garden patterns by adding steps such as *Draw Lines*, and *Place Rock*.
- We added a unique **Auto-Run ZenBot from Camera** mode:
  The robot captures an image via camera, analyzes the garden layout, **auto-generates a sequence of drawing and placement steps**, and immediately executes them.

---

### 3. Technical Implementations

#### Teleoperation / Dataset Capture
We first collected fundamental motion trajectories for creating a Zen garden through teleoperation, including:
 - Raking straight lines in the sand
 - Placing rocks in predefined locations

For the raking actions in particular, we designed and 3D-printed a custom attachment to ensure that the SO-101 could grasp and manipulate the rake robustly. 
(**Acknowledgement**: We would like to express our gratitude to Robostadion for their support in 3D-printing the custom attachment used for raking operations.)

To improve robustness and reduce overfitting during teleoperation, we intentionally placed manipulable objects (a rock and tools) at various positions around the sandbox each time the robot was asked to grasp them. This ensured that the robot experienced diverse initial conditions rather than a fixed setup.

We also used two cameras throughout data collection:
- A wrist-mounted camera attached near the gripper
- An overhead camera providing a full top-down view of the sandbox

Before grasping an object, the teleoperator performed deliberate sweeping motions with the wrist-mounted camera to visually search the surrounding area and locate the target object. After grasping, the operator repositioned the arm so that the overhead camera could clearly observe the grasped object, allowing confirmation of grasp correctness (position and orientation).

These procedures significantly improved the robustness and reliability of object handling in our dataset.


![ZenBot Teleopration](./assets_for_doc/image_zenbot_teleoperation.png)

---

#### Training
Using the teleoperated trajectories we collected, we trained SmolVLA for our Zen garden manipulation tasks.

---

#### Inference
Based on the trained model, the robot performs only primitive actions, such as drawing lines or placing rocks.
The high-level action planning logic is handled outside the model (via the UI or an external agent), which determines the full sequence of operations for creating the Zen garden.
This design choice is intentional: the SmolVLA model we used for training is relatively small, and offloading the role of a high-level orchestrator allows us to achieve more reliable and robust execution. 

---

### 4. Ease of Use

- **Generalizable across environments:**  
  ZenBot operates on any flat drawing surface and can adapt to different sizes of sand beds, rakes, and drawing tools.

- **Flexible architecture:**  
  The step representation is lightweight and easily extended with new operations (e.g., spirals, waves, complex sand patterns).

- **Simple interface:**  
  Users interact only through:
  - Manual buttons (*Draw Lines*, *Draw Circles*, *Place Rock*)  
  - The **Auto-Run ZenBot from Camera** button  
  - The **Start ZenBot** button for execution  

- **No robotics expertise required:**  
  The system abstracts robot control into simple actions, making it approachable even for non-technical users.

---

## Additional Links

- Link to a demo video
  - [ZenBot Demo — Rock Placement](https://youtube.com/shorts/2LpQgoojOZM?si=JQuTWASMso2PRlKe)
  - [ZenBot Demo — Raking Patterns](https://www.youtube.com/watch?v=PLaxTkR1wUs)
- URL of your dataset in Hugging Face
  - [Rock Placement Dataset](https://huggingface.co/datasets/wmeddie/zenbot_place_rock3)
  - [Raking Motion Dataset](https://huggingface.co/datasets/wmeddie/zenbot_rake8)
- URL of your model in Hugging Face
  - [Rock Placement Policy Model](https://huggingface.co/wmeddie/smolvla_place_rock3_from_base)
  - [Raking Policy Model](https://huggingface.co/wmeddie/smolvla_rake8_from_base)

---

## Code Submission

This repository follows the provided directory structure.  
Please place your training code and inference scripts inside:


