---
tags:
  - NCN
  - Demo
---
## Run Sheet
1. Show what I changed in the [PR](https://github.com/WiseTechGlobal/Blazor.Diagrams/pull/44/files)
2. Show last run, start from the [actions](https://github.com/WiseTechGlobal/Blazor.Diagrams/actions) page.
	1. Go into the nuget step
	2. Go to nuget
	3. Search for WTG.Z.Blazor.Diagrams
3. Show that I upgraded the version in Dev repo in the [Dev Pr](https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev/pullrequest/250952?path=%2FDirectory.Packages.props), which is now being checked int

> [!question]
> **Gulam:** are we going to be updating Directory.Packages.Props every time?
> > [!info]
> > Just update Directory.Packages.Props every time. I'm doing the documentation for this right now :) 

# Publishing WTG.Z.Blazor.Diagrams for WTG usage

This doc explains how the package we use at WTG is updated.

## Creating Releases with Github Actions
When a PR is merged to master, the GitHub Action 'Create release' should run. This action creates a GitHub Release and uploads the package to the release. The version number is also handled by the action. The package is created on build in the action and should contain Blazor.Diagrams.dll, Blazor.Diagrams.Core.dll and SvgPathProperties.dll. This action also pushes this release to NuGet, see: [WTG.Z.Blazor.Diagrams](https://proget.wtg.zone/feeds/Gallery/WTG.Z.Blazor.Diagrams/versions).

> [!tip]
> If you have a WTG workitem that requires a push to this repo, you will additionally need to update the WTG.Z.Blazor.Diagraqms version in the WTG Dev repo with the steps below.

## Updating WTG Dev Repo
1. Go to [WTG.Z.Blazor.Diagrams on nuget](https://proget.wtg.zone/feeds/Gallery/WTG.Z.Blazor.Diagrams/versions) and find the latest version number.
2. Navigate to [Directory.Packages.Props](https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FDirectory.Packages.props&version=GBmaster&line=113&lineEnd=113&lineStartColumn=1&lineEndColumn=72&lineStyle=plain&_a=contents) and update the version number in the line `<PackageVersion Include="WTG.Z.Blazor.Diagrams" Version="x.x.x" />` to the latest one.
