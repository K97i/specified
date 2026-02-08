<script lang="ts">

    export let report: any;
    export let eolList: any;

    // EOL Check

    let valid: Boolean = false;
    let latest: Boolean = false;
    let insider: Boolean = false;

let windows11: Boolean = false;
let latestVersions: Array<string> = [];
let validVersions: Array<string> = [];

    function eolCheck() {

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

</script>

<div>
    <h1>General Notes</h1>
    <li>
        The OS is 
        {#if insider}
            <p>Insider</p>
        {:else}
            <span>{ valid ? "Not EOL" : "EOL" }</span>{ (valid && !latest) ? ", but " : "and " }<span>{latest ? "Up-to-date" : "not Up-to-date"}</span>.
        {/if}
    </li>
</div>

<style>
	h1 {
		color: var(--color-secondary-50);
	}
</style>