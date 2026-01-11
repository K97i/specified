<script lang="ts">

    export let report: any;
    export let eolList: any;

    // EOL Check

    function eolCheck() {
        let windows11: Boolean = false;
        let latestVersions: Array<string> = [];
        let validVersions: Array<string> = [];

        let valid: Boolean = false;
        let eol: Boolean = false;
        let insider: Boolean = false;

        eolList.forEach((version: any) => {
            // Windows 11
            if (
                !(version.lts)
                && !(version.cycle.includes("-e"))
                && !(version.cycle.includes("11"))
                && windows11
            ){
                latestVersions.push(version.latest);
                windows11 = true;
            }

            if (
                !(version.lts)
                && new Date(version.support) > new Date()
            ) validVersions.push(version.cycle)
        });

        if (latestVersions.includes(report.BasicInfo.Version)) eol = false;
        if (validVersions.includes(report.BasicInfo.Version)) valid = true;
    }

    /*

    $eoldata = json_decode(file_get_contents('https://endoflife.date/api/windows.json'), true);
    $validversions = '';
    $latestver = '';
    $found10 = false;
    $found11 = false;

    // Set Latest Version
    foreach ($eoldata as $eolitem) {
        // Windows 10
        if (!$eolitem['lts']
            && !str_contains($eolitem['cycle'], '-e')
            && str_contains($eolitem['cycle'], '10')
            && $found10 === false) {
            $latestver = $latestver . $eolitem['latest'] . ' ';
            $found10 = true;
        }

        // Windows 11
        if (!$eolitem['lts']
            && !str_contains($eolitem['cycle'], '-e')
            && str_contains($eolitem['cycle'], '11')
            && $found11 == false) {
            $latestver = $latestver . $eolitem['latest'] . ' ';
            $found11 = true;
        }

        // Break out of loop
        if ($found10 == true && $found11 == true) {
            break;
        }
    }

    foreach ($eoldata as $eolitem) {
        if (!$eolitem['lts']
            && !str_contains($eolitem['cycle'], '-e')
            && strtotime($eolitem['support']) > time()) {
            $validversions = $validversions . $eolitem['latest'] . ' ';
        }
    }

    */
</script>