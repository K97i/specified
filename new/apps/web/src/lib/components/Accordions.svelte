<script lang="ts">
    import type { Report } from "$lib/common/report/report";
	import TabledData from '$lib/common/TabledData.svelte';
	import { Accordion } from '@skeletonlabs/skeleton-svelte';

    let errorTables: Boolean = $state(false);

    interface Props {
		report: Report;
	}

	let {
		report,
	}: Props = $props();

    if (
        report.Events.UnexpectedShutdowns.length > 0 ||
        report.Events.MachineCheckExceptions.length > 0 ||
        report.Events.WheaErrorRecords.length > 0 ||
        report.Events.PciWheaErrors.length > 0
        )
        errorTables = true;
</script>

<!-- Devices, Drivers -->
<div>
    <!-- Devices -->
    <Accordion collapsible>
        <Accordion.Item value='devices'>
            <h3>
                <Accordion.ItemTrigger>Devices</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.Hardware.Devices} headers={Object.keys(report.Hardware.Devices[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    
    <!-- Drivers -->
    <Accordion collapsible>
        <Accordion.Item value='drivers'>
            <h3>
                <Accordion.ItemTrigger>Drivers</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.Hardware.Drivers} headers={Object.keys(report.Hardware.Drivers[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
</div>

<!-- Running Processes, Installed Apps, Installed Windows Store Packages -->
<div>
    <!-- Running Processes -->
    <Accordion collapsible>
        <Accordion.Item value='running-proc'>
            <h3>
                <Accordion.ItemTrigger>Running Processes</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.System.RunningProcesses} headers={Object.keys(report.System.RunningProcesses[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    
    <!-- Installed Apps -->
    <Accordion collapsible>
        <Accordion.Item value='installed-apps'>
            <h3>
                <Accordion.ItemTrigger>Installed Apps</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.System.InstalledApps} headers={Object.keys(report.System.InstalledApps[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    
    <!-- Installed Windows Store Packages -->
    <Accordion collapsible>
        <Accordion.Item value='installed-wsp'>
            <h3>
                <Accordion.ItemTrigger>Installed Windows Store Packages</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.System.WindowsStorePackages} headers={Object.keys(report.System.WindowsStorePackages[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
</div>

<!-- Services, Tasks -->
<div>
    <!-- Services -->
    <Accordion collapsible>
        <Accordion.Item value='services'>
            <h3>
                <Accordion.ItemTrigger>Services</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.System.Services} headers={Object.keys(report.System.Services[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    
    <!-- Tasks -->
    <Accordion collapsible>
        <Accordion.Item value='tasks'>
            <h3>
                <Accordion.ItemTrigger>Tasks</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.System.ScheduledTasks} headers={Object.keys(report.System.ScheduledTasks[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
</div>


<!-- Error Tables -->
{#if errorTables}
<div>
    <!-- Unexpected Shutdowns -->
    {#if report.Events.UnexpectedShutdowns.length > 0}
    <Accordion collapsible>
        <Accordion.Item value='unexp-sd'>
            <h3>
                <Accordion.ItemTrigger>Unexpected Shutdowns</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.System.Services} headers={Object.keys(report.System.Services[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    {/if}
    
    <!-- Machine Check Exceptions -->
    {#if report.Events.MachineCheckExceptions.length > 0}
    <Accordion collapsible>
        <Accordion.Item value='mce'>
            <h3>
                <Accordion.ItemTrigger>Machine Check Exceptions</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.Events.MachineCheckExceptions} headers={Object.keys(report.Events.MachineCheckExceptions[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    {/if}
    
    <!-- WHEA Error Records -->
    {#if report.Events.MachineCheckExceptions.length > 0}
    <Accordion collapsible>
        <Accordion.Item value='whea'>
            <h3>
                <Accordion.ItemTrigger>WHEA Error Records</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.Events.WheaErrorRecords} headers={Object.keys(report.Events.WheaErrorRecords[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    {/if}
    
    <!-- PCI WHEA Errors -->
    {#if report.Events.PciWheaErrors.length > 0}
    <Accordion collapsible>
        <Accordion.Item value='pci-whea'>
            <h3>
                <Accordion.ItemTrigger>PCI WHEA Errors</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.Events.PciWheaErrors} headers={Object.keys(report.Events.PciWheaErrors[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    {/if}
</div>
{/if}

<!-- Network -->
<div>
    <!-- Network Connections -->
    <Accordion collapsible>
        <Accordion.Item value='network-connections'>
            <h3>
                <Accordion.ItemTrigger>Network Connections</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.Network.NetworkConnections} headers={Object.keys(report.Network.NetworkConnections[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    
    <!-- Routes Table -->
    <Accordion collapsible>
        <Accordion.Item value='installed-apps'>
            <h3>
                <Accordion.ItemTrigger>Installed Apps</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <TabledData data={report.Network.Routes} headers={Object.keys(report.Network.Routes[0])} params="paginate search"/>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
    
    <!-- Hosts File -->
    <Accordion collapsible>
        <Accordion.Item value='installed-wsp'>
            <h3>
                <Accordion.ItemTrigger>Installed Windows Store Packages</Accordion.ItemTrigger>
            </h3>
            <Accordion.ItemContent>
                <code>
                    {report.Network.HostsFile}
                </code>
            </Accordion.ItemContent>
        </Accordion.Item>
    </Accordion>
</div>