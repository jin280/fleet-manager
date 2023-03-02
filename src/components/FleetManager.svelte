<script>
    import { onMount } from "svelte";
    import machines from "../api/mockApi.js";

    /**
     * Represents a location.
     * @class Location
     * @param {number} N - North coordinate
     * @param {number} E - East coordinate
     * @param {number} H - Height coordinate
     */
    class Location {
        constructor(N, E, H) {
            this.N = N;
            this.E = E;
            this.H = H;
        }
    }

    /**
     * Represents a machine.
     * @class Machine
     * @param {string} id - unique identifier
     * @param {string} type - type of machine
     * @param {Location} location - location of machine
     * @param {string} make - make of machine
     * @param {boolean} active - whether machine is active or not
     */
    class Machine {
        constructor(id, type, { N, E, H }, make, active) {
            this.id = id;
            this.type = type;
            this.location = new Location(N, E, H);
            this.make = make;
            this.active = active;
        }
    }

    /**
     * Creates an array of Machine objects from the given array of machines.
     * @function createMachineList
     * @param {Array} machines - array of machines
     * @returns {Array} - array of Machine objects
     */
    function createMachineList(machines) {
        let machineList = [];
        machines.forEach((machine) => {
            machineList.push(
                new Machine(
                    machine.id,
                    machine.type,
                    machine.location,
                    machine.make,
                    machine.active
                )
            );
        });
        return machineList;
    }

    /**
     * Assigns a given machine to the selectedMachine variable. If the machine is already selected, it is unselected.
     * @function selectMachine
     * @param {Machine} machine - machine to select
     * @returns {void}
     */
    function selectMachine(machine) {
        selectedMachine = selectedMachine === machine ? null : machine;
    }

    // array of Machine objects
    let machineList = [];
    // selectedMachine is the machine that is currently selected for details
    let selectedMachine = null;

    // Initializes variables on mount
    onMount(() => {
        machineList = createMachineList(machines);
    });
</script>

<div class="fleet-manager">
    <div class="list">
        <!-- Iterate list of machines, if active, and display them -->
        <ul>
            {#each machineList as machine}
                {#if machine.active}
                    <li class:active={selectedMachine === machine}>
                        <button on:click={() => selectMachine(machine)}>
                            {`${machine.type} - ${machine.make}`}
                        </button>
                    </li>
                {/if}
            {/each}
        </ul>
    </div>
    <div class="details">
        <!-- If machine is selected, display details -->
        {#if selectedMachine}
            <p>ID: {selectedMachine.id}</p>
            <p>Type: {selectedMachine.type}</p>
            <p>Make: {selectedMachine.make}</p>
            <p>Active: {selectedMachine.active}</p>
            <p class="location">Location:</p>
            <p>N: {selectedMachine.location.N}</p>
            <p>E: {selectedMachine.location.E}</p>
            <p>H: {selectedMachine.location.H}</p>
        {:else}
            <p>No selection</p>
        {/if}
    </div>
</div>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .list {
        display: flex;
        flex-direction: column;
        margin: 20px;
    }
    .details {
        display: flex;
        flex-direction: column;
        margin: 20px;
        justify-content: center;
        width: 50%;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        padding: 20px;
        margin: 0 10px;
        text-align: left;
    }
    .fleet-manager {
        display: flex;
        flex-direction: row;
        justify-content: center;
    }
    .location {
        font-weight: bold;
        margin-top: 20px;
    }
    .active {
        background-color: #e6e6e6;
        border-radius: 5px;
    }
    button {
        border: none;
        background-color: transparent;
        cursor: pointer;
        font-size: 1.2rem;
        font-weight: bold;
        text-align: left;
    }
    p {
        margin: 0;
    }
</style>
