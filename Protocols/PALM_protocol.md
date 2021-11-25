# Photoactivatable localization microscopy protocol
### A quick note
This is still very much a work of progress. These are the parameters that worked best with the ever-changing demo unit -- not necessarily what will work 
best at the BIF. For a detailed description of PALM, [this paper](https://www.science.org/doi/10.1126/science.1127344) can be referenced, or for a more broad 
overview of how PALM is related to the broader field of super-resolution imaging, [this presentation](../Presentations/20211025_group_meeting.pptx) may be useful.

### Imaging with PALM
At this point, we should have a fixed sample, labelled with photoactivatable dyes. If not, refer to [this staining protocol](/Protocols/staining_protocol.pdf) to learn how to stain your sample, or if you have already stained it, reference [this fixation protocol](../Protocols/fixation_protocol.md) to learn how to fix your cell and label with Hoechst DNA dye if necessary.

#### Acquisition sequence
While it was suggested by Meagan Esbin of the Tjian + Darzacq Group at UC Berkeley that we have the activation laser (405 nm) and aquisition laser (561/647 nm depending on the PA-dye used) turned on simulataneously with the following acquisition/illumination sequence over 20,000 - 30,000 frames.

| Exposure sequence | Camera aquisition | Camera aquisition | Camera aquisition | Camera aquisition | ...             |
| ----------------- | ----------------- | ----------------- | ----------------- | ----------------- | --------------- |
| Activation beam   | 405 nm            | 405 nm            | 405 nm            | 405 nm            | ...             |
| Aquisition beam   | 561/647 nm        | 561/647 nm        | 561/647 nm        | 561/647 nm        | ...             |
| Integration time  | 100 ms            | 100 ms            | 100 ms            | 100 ms            | ...             |

However, due to the demo-microscope unit having an incorrect dichroic, this acquisition/illumination sequence resulted in a lot of bleed-through from the 405 nm beam being on. This meant that we had to modify the sequence to ensure that there was no camera exposure while the 405 nm beam was on. I would suggest something like the following (again over 20,000 to 30,000 frames, or really until no more meaningful fluorescence is detected).

| Exposure sequence |                   | Camera aquisition | Camera aquisition | Camera aquisition | ...             |
| ----------------- | ----------------- | ----------------- | ----------------- | ----------------- | --------------- |
| Activation beam   | 405 nm            |                   |                   |                   | ...             |
| Aquisition beam   |                   | 561/647 nm        | 561/647 nm        | 561/647 nm        | ...             |
| Integration time  | 100 ms            | 100 ms            | 100 ms            | 100 ms            | ...             |

In general, a ratio of activation frames to aquisition frames around 1:10 works quite well. In additon, the integration times are also rather flexible.

### Localization and drift correction
Both localization and drift correction can be done in the proprietary Nikon software, in ImageJ with a package like ThunderSTORM, or with original code (though I would strongly recommend against it). I opted to use the Nikon software as it can do simultaneous aquisition and localization while providing reasonable control over the fitting parameters, but this may not have been the best choice; its still unclear due to a lack of quality datasets to test localization and drift correction on.

### Analysis of localized data
At this point, all going well, we should have a .csv or .xslx containing the coordinates of localization. The data can be imported into [this notebook](../Notebooks/hub_quantification.ipynb) for analysis. If time permits, it would be quite productive to convert it into a script, but for now, a notebook is all I've written. The analysis should produce a probability mass function (PMF) and cumulative distribution function (CDF) of the number of EWS-FLI1 proteins per hub, and the number of hubs. More info can be found in the documentation.
