<script lang="ts">
    export let report: any;
    export let eolList: any;

    // EOL Check

    let valid: Boolean = false, 
        latest: Boolean = false, 
        insider: Boolean = false;

    function eolCheck() {

        let windows11: Boolean = false;
        let latestVersions: Array<string> = [];
        let validVersions: Array<string> = [];

        eolList.forEach((version: any) => {
            // Windows 11
            if (
                !(version.lts)
                && !(version.cycle.includes("-e"))
                && (version.cycle.includes("11"))
                && !windows11
            ){
                latestVersions.push(version.latest);
                windows11 = true;
            }

            if (
                !(version.lts)
                && new Date(version.support) > new Date()
            ) validVersions.push(version.latest)
        });

        if (latestVersions.includes(report.BasicInfo.Version)) latest = true;
        if (validVersions.includes(report.BasicInfo.Version)) valid = true;
        if (validVersions[0].split(".")[2] < report.BasicInfo.Version.split(".")[2]) insider = true;

    }

    eolCheck();

    // Uptime

    const days: Number = Math.floor(report.BasicInfo.Uptime / 60 / 60 / 24),
        hours: Number = Math.floor(report.BasicInfo.Uptime / 60 / 60 % 24), 
        minutes: Number = Math.floor(report.BasicInfo.Uptime / 60 % 60), 
        seconds: Number = report.BasicInfo.Uptime % 60;

</script>

<div>
    <h1>General Notes</h1>
    <li> <!-- OS -->
        The OS is 
        {#if insider}
            <p>Insider</p>
        {:else}
            <span>{ valid ? "Not EOL" : "EOL" }</span>{ (valid && !latest) ? ", but " : "and " }<span>{latest ? "Up-to-date" : "not Up-to-date"}</span>.
        {/if}
        <span>(version {report.BasicInfo.FriendlyVersion}, build {report.BasicInfo.Version})</span>
    </li>
    <li>
        The current uptime is {days} days, {hours} hours, {minutes} minutes, and {seconds} seconds.
    </li>
    <li>
        {#if report.Security.AvList.length > 0}
            Currently Installed AVs are: 
            <span>
                {#each report.Security.AvList as antivirus}
                    {antivirus}
                {/each}
            </span>
        {:else}
            There are no installed AVs!
        {/if}
    </li>
</div>