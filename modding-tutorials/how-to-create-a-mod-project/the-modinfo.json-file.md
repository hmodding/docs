---
description: >-
  This page aims to give you all the needed information about the modinfo.json
  file.
---

# The modinfo.json file

Here is a list of all the fields of the modinfo.json file and a small description/usage of them. More info about them below this table.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field Name</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>name</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String</b>
        </p>
        <p>Description : <b>The display name of your mod.</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>author</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String</b>
        </p>
        <p>Description : <b>Your username.</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>description</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String</b>
        </p>
        <p>Description : <b>A small description of your mod.</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>requiredByAllPlayers</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>Bool</b>
        </p>
        <p>Description : <b>Is the mod required by all players to work?</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>version</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String</b>
        </p>
        <p>Description : <b>The current version of your mod.</b>
        </p>
        <p>Usage : <b>We recommend using</b>  <a href="https://semver.org/"><b>Semantic Versioning 2.0.0</b></a><b>&#x200B;</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>license</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String</b>
        </p>
        <p>Description : <b>The license of your mod.</b>
        </p>
        <p>Usage : <b>You can find many licenses on</b>  <a href="https://choosealicense.com/"><b>choosealicense.com</b></a><b>&#x200B;</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>icon</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String / Path</b>
        </p>
        <p>Description : <b>An icon for your mod. (Can be seen in the mod manager list)</b>
        </p>
        <p>Usage : <b>We recommend you a 512x512 png or jpg image.</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>banner</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String / Path</b>
        </p>
        <p>Description : <b>A banner for your mod. (Can be seen in the mod manager list)</b>
        </p>
        <p>Usage : <b>We recommend you a 660 x 200 png or jpg image.</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>gameVersion</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String</b>
        </p>
        <p>Description : <b>The version of Green Hell that you made was made for.</b>
        </p>
        <p>Usage : <b>This is just for info, Mods should remain compatible across versions</b>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>updateUrl</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>String / Url</b>
        </p>
        <p>Description : <b>A link that returns the latest available version of your mod.</b>
        </p>
        <p>Usage : <b>Our site provides you an url when you release your mod.</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>isModPermanent</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>Boolean (true or false)</b>
        </p>
        <p>Description : <b>Defines if your mod is permanent. Permanent mods are loaded by default and can&apos;t be unloaded. This is needed when mods add new items/blocks that can&apos;t really be unloaded without causing issues.</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>excludedFiles</b>
      </td>
      <td style="text-align:left">
        <p>Type : <b>Array of strings / List of strings</b>
        </p>
        <p>Description : <b>Allows you to specify files to not load. This doesn&apos;t support wildcards yet.</b>
        </p>
        <p>Usage : <b>Useful when you want to exclude files like readme or source files.</b>
        </p>
      </td>
    </tr>
  </tbody>
</table>

 **Icon & Banner Fields :**   
**Just add an image to your solution folder where your** **`.cs`** **files and the** **`.csproj`** **file is and edit the** **`modinfo.json`** **file as shown below.**

![](https://gblobscdn.gitbook.com/assets%2F-M5KKfkIqMO_EFPortVm%2F-M5eDSsjwiUWaLaLvoJ2%2F-M5eLQxSe2MfTv4wdoCA%2Fimage.png?alt=media&token=1dde7eaa-2c04-457f-8dba-f6bb9104b52d)

**UpdateUrl Field :   
RaftModLoader fetch this link to know what is the latest available version of your mod, if the currently installed version is not equal to the version returned by this url it will say that the mod is outdated.  
Our website offer this service with a nice automation system. Available on the following link once you have a mod slug.**  
 `https://www.raftmodding.com/api/v1/mods/`**`YOURMODSLUG`**`/version.txt`

**requiredByAllPlayers Field :  
If this field is set to true and the mod is loaded it will kick any player attempting to join that does not have the same mod and the same version. If your mod adds new items or new blocks, this definitely needs to be true! So nobody will be able to join with missing blocks!**

**ExcludedFiles Field :   
This is a simple list of excluded files as shown below.**

![](https://gblobscdn.gitbook.com/assets%2F-M5KKfkIqMO_EFPortVm%2F-M5eDSsjwiUWaLaLvoJ2%2F-M5eMfiNdAlpQcRRqPZ9%2Fimage.png?alt=media&token=a8ed217b-34d5-4e8d-aa92-b0ca01f6f0f2)

