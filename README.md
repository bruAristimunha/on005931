[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on005931-blue)](https://doi.org/10.82901/nemar.on005931)

Dataset of intracranial EEG from human epilepsy patients performing a visuomotor task

Description:

We present an electrophysiological dataset recorded from ten subjects during a visuomotor task. Subjects were epilepsy patients undergoing intracranial monitoring for localization of epileptic seizures. Subjects completed five sessions of Speed Match - a visuomotor on the Lumosity platform (https://www.lumosity.com/; Lumos Labs, Inc, San Francisco, CA) - during interictal EEG recording.

Repository structure:

Main directory (interictal EEG from children during gameplay): Contains interictal EEG files of each participant in the study. Folders are explained below.

Subfolders:

sub-/: Contains folders for each subject, named sub-.
sub-/ses-: Contains folders for visuomotor task.
sub-/ses-/ieeg/: Contains the raw iEEG data in .edf format for each subject. Each subject performed visuomotor task (ses-task). Each *ieeg.edf file contains continuous iEEG data during the visuomotor task. Details about the channels are given in the corresponding .tsv file. We also provide the information on the timing of the finger tapping on ieeg/edf file by specifying the start and end sample of each trial. (101 is for finger tapping). 

## Channel names follow the data headers (18 Oct 2026)

In 16 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count). In the 16 with a different channel count, `mne_bids.read_raw_bids` warned and kept the header names, so the channels.tsv types and bad-channel status reached only the channels whose names happen to be identical; a reader that applies the channels.tsv rows by position gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 990 rows took the header name (the header labels carry 'POL ' prefix + '-AV' suffix).

990 channels.tsv rows had no channel in the data file (16 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-01_ses-01_task-game_run-01_ieeg.edf`: B1, B10, B11, B12, B13; `sub-01_ses-02_task-game_run-01_ieeg.edf`: A1, A10, A11, A12, A13; `sub-02_ses-01_task-game_run-01_ieeg.edf`: B1, B10, B11, B12, B13.

16 files listed the channels in another order than the header; rows now follow the header order.

Applied by position, the former rows would have given wrong labels to 982 channels in 16 files (rows missing, extra or out of order before that position).

electrodes.tsv is unchanged: it names the contacts (e.g. `A1`), while channel names follow the recorder (e.g. `A1-AV`); to attach positions in MNE, map channel names to contact names first, e.g. `raw.rename_channels(lambda n: re.sub(r'-[A-Za-z0-9]+$', '', n))`.

## Electrode locations (19 Oct 2026)

Add coordsystem.json MNI305/mm next to 16 electrodes.tsv (proposal A1, Lane G round 4). Evidence: Wayne State (Asano/Kanno/Kochi) deposits use FreeSurfer-standard (MNI305/fsaverage) electrode coordinates; MNI305 is a millimetre space by definition. No OpenNeuro S3 version of this dataset ever held a coordsystem.json (all versions listed 2026-10-19). Paper PMC12829438 (section 2.2): FreeSurfer used "to normalize each electrode site to a Montreal Neurological Institute (MNI)-standard brain coordinate" (FreeSurfer's MNI space is MNI305); 10 of the 16 electrodes files are byte-identical to ds006234/on006233 (README MNI-305) and 8 to on007119 (fsaverage / mm).

Full evidence per dataset: REPORT-ieeg-locations.md and ieeg_locations.json (Lane G, 19 Oct 2026). This dataset mirrors OpenNeuro; the same change belongs upstream. Local branch only.
