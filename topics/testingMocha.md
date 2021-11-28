# Scenario

```js
/*
	# Logic

		There is a set of Power Plants and a set of Households. Every Household can be
		connected to any number of Power Plants. Power Plant feeds the Household with the
		Electricity. The Household has Electricity if it's connected to one or more
		Power Plants.

		Each Power Plant is alive by default, but can be killed. The Power Plant which
		is not Alive will not generate any Electricity.

		Household can be connected to Household. The Household which has the Electricity
		also passes it to all the connected Households.

		The Power Plant can be repaired after killed.
*/

//	This class is just a facade for your implementation, the tests below are using the `World` class only.
//	Feel free to add the data and behavior, but don't change the public interface.

class World {
    constructor() {}

    createPowerPlant() {
        throw new Error("Not Implemented");
    }

    createHousehold() {
        throw new Error("Not Implemented");
    }

    connectHouseholdToPowerPlant(household, powerPlant) {
        throw new Error("Not Implemented");
    }

    connectHouseholdToHousehold(household1, household2) {
        throw new Error("Not Implemented");
    }

    disconnectHouseholdFromPowerPlant(household, powerPlant) {
        throw new Error("Not Implemented");
    }

    killPowerPlant(powerPlant) {
        throw new Error("Not Implemented");
    }

    repairPowerPlant(powerPlant) {
        throw new Error("Not Implemented");
    }

    householdHasEletricity(household) {
        throw new Error("Not Implemented");
    }
}

const assert = {
    equal(a, b) {
        if (a != b) {
            throw new Error("Assertion Failed");
        }
    },
};

mocha.setup("bdd");

describe("Households + Power Plants", function () {
    it("Household has no electricity by default", () => {
        const world = new World();
        const household = world.createHousehold();
        assert.equal(world.householdHasEletricity(household), false);
    });

    it("Household has electricity if connected to a Power Plant", () => {
        const world = new World();
        const household = world.createHousehold();
        const powerPlant = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household, powerPlant);

        assert.equal(world.householdHasEletricity(household), true);
    });

    it("Household won't have Electricity after disconnecting from the only Power Plant", () => {
        const world = new World();
        const household = world.createHousehold();
        const powerPlant = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household, powerPlant);

        assert.equal(world.householdHasEletricity(household), true);

        world.disconnectHouseholdFromPowerPlant(household, powerPlant);
        assert.equal(world.householdHasEletricity(household), false);
    });

    it("Household will have Electricity as long as there's at least 1 alive Power Plant connected", () => {
        const world = new World();
        const household = world.createHousehold();

        const powerPlant1 = world.createPowerPlant();
        const powerPlant2 = world.createPowerPlant();
        const powerPlant3 = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household, powerPlant1);
        world.connectHouseholdToPowerPlant(household, powerPlant2);
        world.connectHouseholdToPowerPlant(household, powerPlant3);

        assert.equal(world.householdHasEletricity(household), true);

        world.disconnectHouseholdFromPowerPlant(household, powerPlant1);
        assert.equal(world.householdHasEletricity(household), true);

        world.killPowerPlant(powerPlant2);
        assert.equal(world.householdHasEletricity(household), true);

        world.disconnectHouseholdFromPowerPlant(household, powerPlant3);
        assert.equal(world.householdHasEletricity(household), false);
    });

    it("Household won't have Electricity if the only Power Plant dies", () => {
        const world = new World();
        const household = world.createHousehold();
        const powerPlant = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household, powerPlant);

        assert.equal(world.householdHasEletricity(household), true);

        world.killPowerPlant(powerPlant);
        assert.equal(world.householdHasEletricity(household), false);
    });

    it("PowerPlant can be repaired", () => {
        const world = new World();
        const household = world.createHousehold();
        const powerPlant = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household, powerPlant);

        assert.equal(world.householdHasEletricity(household), true);

        world.killPowerPlant(powerPlant);
        assert.equal(world.householdHasEletricity(household), false);

        world.repairPowerPlant(powerPlant);
        assert.equal(world.householdHasEletricity(household), true);

        world.killPowerPlant(powerPlant);
        assert.equal(world.householdHasEletricity(household), false);

        world.repairPowerPlant(powerPlant);
        assert.equal(world.householdHasEletricity(household), true);
    });

    it("Few Households + few Power Plants, case 1", () => {
        const world = new World();

        const household1 = world.createHousehold();
        const household2 = world.createHousehold();

        const powerPlant1 = world.createPowerPlant();
        const powerPlant2 = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household1, powerPlant1);
        world.connectHouseholdToPowerPlant(household1, powerPlant2);
        world.connectHouseholdToPowerPlant(household2, powerPlant2);

        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), true);

        world.killPowerPlant(powerPlant2);
        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), false);

        world.killPowerPlant(powerPlant1);
        assert.equal(world.householdHasEletricity(household1), false);
        assert.equal(world.householdHasEletricity(household2), false);
    });

    it("Few Households + few Power Plants, case 2", () => {
        const world = new World();

        const household1 = world.createHousehold();
        const household2 = world.createHousehold();

        const powerPlant1 = world.createPowerPlant();
        const powerPlant2 = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household1, powerPlant1);
        world.connectHouseholdToPowerPlant(household1, powerPlant2);
        world.connectHouseholdToPowerPlant(household2, powerPlant2);

        world.disconnectHouseholdFromPowerPlant(household2, powerPlant2);

        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), false);

        world.killPowerPlant(powerPlant2);
        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), false);

        world.killPowerPlant(powerPlant1);
        assert.equal(world.householdHasEletricity(household1), false);
        assert.equal(world.householdHasEletricity(household2), false);
    });

    it("Household + Power Plant, case 1", () => {
        const world = new World();

        const household = world.createHousehold();
        const powerPlant = world.createPowerPlant();

        assert.equal(world.householdHasEletricity(household), false);
        world.killPowerPlant(powerPlant);

        world.connectHouseholdToPowerPlant(household, powerPlant);

        assert.equal(world.householdHasEletricity(household), false);
    });
});

describe("Households + Households + Power Plants", function () {
    it("2 Households + 1 Power Plant", () => {
        const world = new World();

        const household1 = world.createHousehold();
        const household2 = world.createHousehold();
        const powerPlant = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household1, powerPlant);
        world.connectHouseholdToHousehold(household1, household2);

        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), true);

        world.killPowerPlant(powerPlant);

        assert.equal(world.householdHasEletricity(household1), false);
        assert.equal(world.householdHasEletricity(household2), false);
    });

    it("Power Plant -> Household -> Household -> Household", () => {
        const world = new World();

        const household1 = world.createHousehold();
        const household2 = world.createHousehold();
        const household3 = world.createHousehold();
        const powerPlant = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household1, powerPlant);
        world.connectHouseholdToHousehold(household1, household2);
        world.connectHouseholdToHousehold(household2, household3);

        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), true);
        assert.equal(world.householdHasEletricity(household3), true);

        world.killPowerPlant(powerPlant);

        assert.equal(world.householdHasEletricity(household1), false);
        assert.equal(world.householdHasEletricity(household2), false);
        assert.equal(world.householdHasEletricity(household3), false);

        world.repairPowerPlant(powerPlant);

        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), true);
        assert.equal(world.householdHasEletricity(household3), true);

        world.disconnectHouseholdFromPowerPlant(household1, powerPlant);

        assert.equal(world.householdHasEletricity(household1), false);
        assert.equal(world.householdHasEletricity(household2), false);
        assert.equal(world.householdHasEletricity(household3), false);
    });

    it("2 Households + 2 Power Plants", () => {
        const world = new World();

        const household1 = world.createHousehold();
        const household2 = world.createHousehold();

        const powerPlant1 = world.createPowerPlant();
        const powerPlant2 = world.createPowerPlant();

        world.connectHouseholdToPowerPlant(household1, powerPlant1);
        world.connectHouseholdToPowerPlant(household2, powerPlant2);

        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), true);

        world.killPowerPlant(powerPlant1);

        assert.equal(world.householdHasEletricity(household1), false);
        assert.equal(world.householdHasEletricity(household2), true);

        world.connectHouseholdToHousehold(household1, household2);

        assert.equal(world.householdHasEletricity(household1), true);
        assert.equal(world.householdHasEletricity(household2), true);

        world.disconnectHouseholdFromPowerPlant(household2, powerPlant2);

        assert.equal(world.householdHasEletricity(household1), false);
        assert.equal(world.householdHasEletricity(household2), false);
    });
});

mocha.run();
```

# Implementation 1 - functional

https://jsfiddle.net/oxfrkc7t

```js
class World {
    constructor() {
        this.households = [];
        this.powerPlants = [];
        this.houseToPlantConnections = [];
        this.houseToHouseConnections = [];
    }

    createPowerPlant() {
        let powerPlant = {
            id: this.powerPlants.length + 1,
            isAlive: true,
        };
        this.powerPlants.push(powerPlant);
        return powerPlant;
    }

    createHousehold() {
        let household = {
            id: this.households.length + 1,
        };
        this.households.push(household);
        return household;
    }

    connectHouseholdToPowerPlant(household, powerPlant) {
        this.houseToPlantConnections.push({
            houseId: household.id,
            plantId: powerPlant.id,
        });
    }

    connectHouseholdToHousehold(household1, household2) {
        this.houseToHouseConnections.push({
            houseOneId: household1.id,
            houseTwoId: household2.id,
        });
    }

    disconnectHouseholdFromPowerPlant(household, powerPlant) {
        this.houseToPlantConnections = this.houseToPlantConnections.filter((connection) => connection.houseId != household.id || connection.plantId != powerPlant.id);
    }

    killPowerPlant(powerPlant) {
        powerPlant.isAlive = false;
        this.powerPlants.map((plant) => {
            if (plant.id == powerPlant.id) {
                plant.isAlive = false;
            }
        });
    }

    repairPowerPlant(powerPlant) {
        powerPlant.isAlive = true;
        this.powerPlants.map((plant) => {
            if (plant.id == powerPlant.id) {
                plant.isAlive = true;
            }
        });
    }

    householdHasEletricity(household) {
        let poweredHouses = [];

        // find powered houses with a direct connection to a live plant
        this.houseToPlantConnections.map((connection) => {
            let houseId = connection.houseId;
            let plantId = connection.plantId;
            let plant = this.powerPlants.filter((plant) => plant.id == plantId)[0];

            if (!poweredHouses.includes(houseId) && plant.isAlive) {
                poweredHouses.push(houseId);
            }
        });

        // check houses with a connection to a powered house
        this.houseToHouseConnections.map((connection) => {
            let houseOneId = connection.houseOneId;
            let houseTwoId = connection.houseTwoId;

            if (!poweredHouses.includes(houseTwoId) && poweredHouses.includes(houseOneId)) poweredHouses.push(houseTwoId);
            if (!poweredHouses.includes(houseOneId) && poweredHouses.includes(houseTwoId)) poweredHouses.push(houseOneId);
        });

        let hasElectricity = poweredHouses.includes(household.id);

        return hasElectricity;
    }
}
```

# Implementation 2 - OOP

```js
class ElectricityNetworkPeer {
    producesElectricity() {
        throw new Error("Not Implemented");
    }
    canTransferElectricity() {
        throw new Error("Not Implemented");
    }
}

class Household extends ElectricityNetworkPeer {
    producesElectricity() {
        return false;
    }
    canTransferElectricity() {
        return true;
    }
}

class PowerPlant extends ElectricityNetworkPeer {
    constructor() {
        super();
        this._alive = true;
    }

    setAlive(alive) {
        this._alive = alive;
    }

    producesElectricity() {
        return this._alive;
    }

    canTransferElectricity() {
        return false;
    }
}

class ElectricityNetwork {
    constructor() {
        this._links = [];
    }

    connectPeers(entityA, entityB) {
        this._links.push([entityA, entityB]);
    }

    breakConnection(entityA, entityB) {
        this._links = this._links.filter((item) => !(item.includes(entityA) && item.includes(entityB)));
    }

    allPeersConnectedTo(entity) {
        const collectPeersFor = (entity, traversedItems = []) => {
            const relatedLinks = [...this._links.filter((item) => item.includes(entity))];

            const connectedPeers = relatedLinks.map((link) => link.find((item) => item != entity)).filter((peer) => !traversedItems.includes(peer));

            traversedItems.push(...connectedPeers);

            return [...connectedPeers].concat(...connectedPeers.filter((peer) => peer.canTransferElectricity()).map((item) => collectPeersFor(item, traversedItems)));
        };

        return [...new Set(collectPeersFor(entity))];
    }
}

//	This class is just a facade for your implementation, the tests below are using the `World` class only.
//	Feel free to add the data and behavior, but don't change the public interface.

class World {
    constructor() {
        this._network = new ElectricityNetwork();
    }

    createPowerPlant() {
        return new PowerPlant();
    }

    createHousehold() {
        return new Household();
    }

    connectHouseholdToPowerPlant(household, powerPlant) {
        this._network.connectPeers(household, powerPlant);
    }

    connectHouseholdToHousehold(household1, household2) {
        this._network.connectPeers(household1, household2);
    }

    disconnectHouseholdFromPowerPlant(household, powerPlant) {
        this._network.breakConnection(household, powerPlant);
    }

    killPowerPlant(powerPlant) {
        powerPlant.setAlive(false);
    }

    repairPowerPlant(powerPlant) {
        powerPlant.setAlive(true);
    }

    householdHasEletricity(household, requestId) {
        return this._network.allPeersConnectedTo(household).some((peer) => peer.producesElectricity());
    }
}
```
