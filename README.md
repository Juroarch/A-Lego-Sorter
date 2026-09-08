# A-Lego-Sorter
Everyone enjoys assembling Lego sets—unless you have to dig the bricks out of endless boxes first. That’s why I wondered: how hard could it be to build a machine that takes any given Lego brick and sorts it into a specific set? Turns out, it’s pretty hard. Here, I’m documenting my progress in building such a system.

## Hardware

For the hardware I am planning to use a 3D-printed machine heavily inspired by Spencer's Sorter V2 (https://github.com/basicallysource/sorter-v2), although I am planning to make the machine way smaller and less professional. 

Components:
- Raspberry Pi 3B+
- Raspberry Pi Camera Module v3
- Arduino Uno (for motor steering and mechanics)

## Sorting Strategy

To keep the machine compact, I am using a multi-pass sorting logic instead of building dozens of physical output bins:

1. First Round: The machine sorts the bricks into 5 to 9 primary containers. At this stage, each container holds a mix of bricks from 2 to 9 different LEGO sets.
2. Second Round: I take the contents of one container, feed them back into the machine, and the sorter distributes them into the final boxes for each specific set.

## Software & AI Detection

To avoid the impossible task of training a classification network on over 20,000 unique LEGO part classes, the system uses a vector-search approach:

- **Model:** A finetuned version of Meta's DINOv2.
- **Training:** The model is trained on a mix of synthetic images generated in Blender and real physical photographs to bridge the reality gap.
- **Infrastructure:** The AI does not run on the Raspberry Pi. Instead, the Pi captures the image and streams it to an external laptop equipped with a dedicated GPU.
- **Logic:** DINOv2 processes the incoming image and generates a characteristic feature vector (embedding). This vector is then compared against a pre-computed database using vector similarity (like Cosine Similarity or KNN). 
- **Database:** The vector database map matches the embeddings directly to official LDraw part IDs.

## License and Contributions

This is a private hobby project. It is licensed under the Alcatraz License (following the "Don't be a dick" principles).

You are completely free to use, modify, and replicate this sorter. However, if you figure out a way to make the machine faster, the detection more accurate, or the 3D parts more stable, it would be highly appreciated if you publish your improvements so the community can benefit from them.
