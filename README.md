# Brainlife App for MNE-Python event detection

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Detects events in the stim channel of continuous MEG/EEG data with mne.find_events and writes them to a BIDS events.tsv with a plot and an MNE report.**

Loads a raw .fif file, runs mne.find_events with the configured options (stim channel, onset/offset, minimum duration, mask and so on), saves the events as a BIDS-compliant events.tsv, and adds an events plot to an MNE HTML report and the product thumbnail. Use it to inspect triggers before epoching. Note: the report is written to out_report/report.html while the registered output is out_dir_report.

| | |
|---|---|
| 📥 **Inputs** | `neuro/meeg/mne/raw` |
| 📤 **Outputs** | `report/html` · `neuro/meg/fif-override` |
| 🐳 **Container** | `brainlifemeeg/mne:1.12.1` |
| ⚙️ **Resources** | 1 CPUs |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.735](https://doi.org/10.25663/brainlife.app.735) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `fif` | `neuro/meeg/mne/raw` | yes | Continuous MEG/EEG recording in MNE raw .fif format. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir_report` | `report/html` | MNE HTML report with a plot of the detected events. |
| `out_dir` | `neuro/meg/fif-override` | BIDS events.tsv with the detected events. |

<details>
<summary><b>⚙️ Parameters</b> (9)</summary>

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

📚 **References used by this App:**
- Gramfort A, et al. MEG and EEG data analysis with MNE-Python. Front Neurosci 7(267):1-13 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)

<!-- brainlife-app:end -->

[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.735-blue.svg)](https://doi.org/10.25663/brainlife.app.735)

## Description

This App loads a `.fif` file containing continuous MEG/EEG data and finds events using
[`mne.find_events`](https://mne.tools/stable/generated/mne.find_events.html). The detected
events are saved to a BIDS-compliant `.tsv` file, plotted with
[`mne.viz.plot_events`](https://mne.tools/stable/generated/mne.viz.plot_events.html), and
summarized in an MNE Report.

The app generates:
- A BIDS-compliant `events.tsv` file with the detected events.
- An MNE Report (`report.html`) with a plot of the detected events.
- A `product.json` summary with a thumbnail of the events plot for the brainlife.io UI.

## Inputs

- **`mne`** (`neuro/meeg/mne/raw`): continuous MEG/EEG data in which to detect events (required).

## Outputs

- **`out_dir/events.tsv`** (`neuro/meg/fif-override`, tag `events`): BIDS-compliant `.tsv` file
  containing the detected events.
- **`out_report/report.html`** (`report/html`): MNE Report containing a plot of the detected
  events.
- **`out_figs/events.png`**: plot of the detected events, embedded in `product.json` for preview
  in the brainlife.io UI.

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

## Usage

### Running on Brainlife.io

1. Go to the app page on brainlife.io (linked by the badge above).
2. Select a `neuro/meeg/mne/raw` dataset as the `mne` input.
3. Adjust the configuration parameters if needed (all have sensible defaults).
4. Submit the process and, once finished, retrieve `out_dir/events.tsv` and
   `out_report/report.html`.

### Local Testing

```bash
git clone --recurse-submodules <repo-url> eventslog
cd eventslog
# edit config.json with the path to your own .fif file and desired parameters
./main
```

## Technical Details

This App uses the `mne.find_events` function from the MNE-Python library to detect events.

## Authors

- Maximilien Chaumon (https://github.com/dnacombo)
- Kamilya Salibay (https://github.com/KSalibay)
- Guiomar Niso (https://github.com/guiomar)

## Citations

- Hayashi, S., Caron, B.A., Heinsfeld, A.S. et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. Nat Methods 21, 809–813 (2024). https://doi.org/10.1038/s41592-024-02237-2
- Gramfort, A. et al. MEG and EEG data analysis with MNE-Python. Front. Neurosci. 7, 267 (2013). https://doi.org/10.3389/fnins.2013.00267

## Funding Acknowledgement

brainlife.io is publicly funded and for the sustainability of the project it is helpful to
Acknowledge the use of the platform. We kindly ask that you acknowledge the funding below in your
code and publications.

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://grantome.com/grant/NIH/R01-EB029272-01)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://grantome.com/grant/NIH/R01-EB030896-01)

## License

Copyright (c) 2026 MEEG Brainlife team. Licensed under AGPL-3.0, see [license.txt](license.txt).
