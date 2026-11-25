<h1 align="center">Show events in file</h1>

<p align="center">
  Detects events in the stim channel of continuous MEG/EEG data with mne.find_events and writes them to a BIDS events.tsv with a plot and an MNE report.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.735"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.735-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-see_LICENSE-lightgrey.svg" alt="License"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Detects events in the stim channel of continuous MEG/EEG data with mne.find_events and writes them to a BIDS events.tsv with a plot and an MNE report.**

Loads a raw .fif file, runs mne.find_events with the configured options (stim channel, onset/offset, minimum duration, mask and so on), saves the events as a BIDS-compliant events.tsv, and adds an events plot to an MNE HTML report and the product thumbnail. Use it to inspect triggers before epoching. Note: the report is written to out_report/report.html while the registered output is out_dir_report.

| | |
|---|---|
| **Inputs** | `neuro/meeg/mne/raw` |
| **Outputs** | `report/html` · `neuro/meg/fif-override` |
| **Container** | `brainlifemeeg/mne:1.12.1` |
| **Resources** | 1 CPUs |
| **Run it** | [doi.org/10.25663/brainlife.app.735](https://doi.org/10.25663/brainlife.app.735) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `fif` | `neuro/meeg/mne/raw` | yes | Continuous MEG/EEG recording in MNE raw .fif format. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir_report` | `report/html` | MNE HTML report with a plot of the detected events. |
| `out_dir` | `neuro/meg/fif-override` | BIDS events.tsv with the detected events. |

<details>
<summary><b>Parameters</b> (9)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `stim_channel` | string |  | `None` \| `str` \| `list of str` Name of the stim channel or all the stim channels affected by triggers. If None, the config variables ‘MNE_STIM_CHANNEL’, ‘MNE_STIM_CHANNEL_1’, ‘MNE_STIM_CHANNEL_2’, etc. are read. If these are not found, it will fall back to ‘STI 014’ if present, then fall back to the first channel of type ‘stim’, if present. If multiple channels are provided then the returned events are the union of all the events extracted from individual stim channels. |
| `output` | string | `onset` | ‘onset’ \| ‘offset’ \| ‘step’ Whether to report when events start, when events end, or both. |
| `consecutive` | string | `increasing` | `bool` \| ‘increasing’ If True, consider instances where the value of the events channel changes without first returning to zero as multiple events. If False, report only instances where the value of the events channel changes from/to zero. If ‘increasing’, report adjacent events only when the second event code is greater than the first. |
| `min_duration` | number | `0` | `float` The minimum duration of a change in the events channel required to consider it as an event (in seconds). |
| `shortest_event` | number | `2` | `int` Minimum number of samples an event must last (default is 2). If the duration is less than this an exception will be raised. |
| `mask` | number | `0` | `int` \| `None` The value of the digital mask to apply to the stim channel values. If None (default), no masking is performed. |
| `uint_cast` | boolean | `false` | `bool` If True (default False), do a cast to uint16 on the channel data. This can be used to fix a bug with STI101 and STI014 in Neuromag acquisition setups that use channel STI016 (channel 16 turns data into e.g. -32768), similar to mne_fix_stim14 --32 in MNE-C. |
| `mask_type` | string | `and` | ‘and’ \| ‘not_and’ The type of operation between the mask and the trigger. Choose ‘and’ (default) for MNE-C masking behavior. |
| `initial_event` | boolean | `false` | `bool` If True (default False), an event is created if the stim channel has a value different from 0 as its first sample. This is useful if an event at t=0s is present. |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.735](https://doi.org/10.25663/brainlife.app.735), choose `neuro/meeg/mne/raw` from your project, set the parameters and press **Execute**. The results are saved to your project as `report/html`, `neuro/meg/fif-override`.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) (the App runs in `brainlifemeeg/mne:1.12.1`), [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository.
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "mne": "/path/to/fif/mne",
       "stim_channel": "",
       "output": "onset",
       "consecutive": "increasing",
       "min_duration": 0,
       "shortest_event": 2,
       "mask": 0,
       "uint_cast": false,
       "mask_type": "and",
       "initial_event": false
   }
   ```

3. Run `./main`.

## Configuration Parameters

| key | type | default | description |
|---|---|---|---|
| `stim_channel` | `None` \| `str` \| `list of str` | `None` | Name of the stim channel or all the stim channels affected by triggers. If `None`, the config variables 'MNE_STIM_CHANNEL', 'MNE_STIM_CHANNEL_1', 'MNE_STIM_CHANNEL_2', etc. are read. If these are not found, it will fall back to 'STI 014' if present, then fall back to the first channel of type 'stim', if present. If multiple channels are provided then the returned events are the union of all the events extracted from individual stim channels. |
| `output` | `'onset'` \| `'offset'` \| `'step'` | `'onset'` | Whether to report when events start, when events end, or both. |
| `consecutive` | `bool` \| `'increasing'` | `'increasing'` | If `True`, consider instances where the value of the events channel changes without first returning to zero as multiple events. If `False`, report only instances where the value of the events channel changes from/to zero. If `'increasing'`, report adjacent events only when the second event code is greater than the first. |
| `min_duration` | `float` | `0` | The minimum duration of a change in the events channel required to consider it as an event (in seconds). |
| `shortest_event` | `int` | `2` | Minimum number of samples an event must last. If the duration is less than this an exception will be raised. |
| `mask` | `int` \| `None` | `None` | The value of the digital mask to apply to the stim channel values. If `None`, no masking is performed. |
| `uint_cast` | `bool` | `False` | If `True`, do a cast to `uint16` on the channel data. This can be used to fix a bug with STI101 and STI014 in Neuromag acquisition setups that use channel STI016 (channel 16 turns data into e.g. -32768), similar to `mne_fix_stim14 --32` in MNE-C. |
| `mask_type` | `'and'` \| `'not_and'` | `'and'` | The type of operation between the mask and the trigger. `'and'` is the MNE-C masking behavior. |
| `initial_event` | `bool` | `False` | If `True`, an event is created if the stim channel has a value different from 0 as its first sample. Useful if an event at t=0s is present. |
| `verbose` | `bool` \| `str` \| `int` \| `None` | `None` | Controls the verbosity of the logging output when detecting events. |

## Local Testing

```bash
git clone --recurse-submodules <repo-url> eventslog
cd eventslog
# edit config.json with the path to your own .fif file and desired parameters
./main
```

## Technical Details

This App uses the `mne.find_events` function from the MNE-Python library to detect events.

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- Show events in file. brainlife.io App. [doi:10.25663/brainlife.app.735](https://doi.org/10.25663/brainlife.app.735)
- Gramfort A, et al. MEG and EEG data analysis with MNE-Python. Front Neurosci 7(267):1-13 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- Maximilien Chaumon (https://github.com/dnacombo)
- Kamilya Salibay (https://github.com/KSalibay)
- Guiomar Niso (https://github.com/guiomar)

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io, which runs this App, is publicly funded by Wellcome, the U.S. National Institutes of Health and the U.S. National Science Foundation; acknowledging that support helps keep it running.</sub>

## License

See [LICENSE](LICENSE).
