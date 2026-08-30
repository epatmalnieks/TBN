<template>
  <v-app>
    <v-tabs vertical dark background-color="primary">
      <v-tab
        v-for="(team, index) in teams"
        :key="index"
        :class="{ 'division-divider': index === 3 || index === 7 }"
      >
        <v-img contain height="30" width="30" :src="getLogo(team.logo)"></v-img>
        <span class="team-name">{{ team.name }}</span>
      </v-tab>
      <v-tab-item v-for="team in teams" :key="team.name">
        <v-row>
          <v-col cols="5">
            <team-table :team="team"></team-table>
          </v-col>
          <v-col cols="6">
            <v-img
              contain
              height="200"
              class="mt-4"
              :src="getLogo(team.logo)"
            ></v-img>
            <p class="d-flex justify-center">{{ team.owner }}</p>
            <div class="d-flex justify-center mt-6 calculations">
              <v-card elevation="15" outlined class="pa-4">
                <p class="mb-2">
                  Starting Salary Cap =
                  {{ formatPrice(team.startingSalaryCap) }}
                </p>
                <p class="mb-2">
                  Total Player Salary =
                  {{ formatPrice(getTotalPlayerSalary(team.players)) }}
                </p>
                <p class="mb-2">
                  Salary Cap Remaining =
                  <span
                    :class="{ 'red--text': getSalaryCapRemaining(team) < 0 }"
                    >{{ formatPrice(getSalaryCapRemaining(team)) }}</span
                  >
                </p>
                <p class="mb-2">Max Bid = {{ formatPrice(getMaxBid(team)) }}</p>
                <p class="mb-1">
                  Positions Remaining = {{ getPositionsRemaining(team) }}
                </p>
              </v-card>
            </div>
          </v-col>
        </v-row>
      </v-tab-item>
      <div class="d-flex justify-center mt-16">
        <v-btn width="100px" color="error" @click="clearClicked">Clear</v-btn>
      </div>
    </v-tabs>
  </v-app>
</template>

<script>
import TeamTable from './components/TeamTable.vue';

export default {
  components: {
    TeamTable,
  },

  data() {
    return {
      selectedTeam: '',

      teams: [
        {
          logo: 'rubberDuckies',
          name: 'Rubber Duckies',
          owner: 'Pete Waskul',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'touchOfClass',
          name: 'Touch of Class',
          owner: 'Tony King',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'endzoneOutlaws',
          name: 'Endzone Outlaws',
          owner: 'Gary King',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'agsOffice',
          name: "AG's Office",
          owner: 'Gavin Holmes',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'atomicDropass',
          name: 'Atomic Dropass',
          owner: 'Will Webb',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'rumspringa',
          name: 'Rumspringa',
          owner: 'Mark Sobiechowski',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'fightingAmish',
          name: 'Fighting Amish',
          owner: 'Dan Sobiechowski',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'finkleIsEinhorn',
          name: 'Finkle is Einhorn!',
          owner: 'Erik Patmalnieks',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'detroitHopheads',
          name: 'Detroit HopHeads',
          owner: 'Jason Webb',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'blitzkrieg',
          name: 'Blitzkrieg',
          owner: 'Eric McEvoy',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'barkingSpiders',
          name: 'Barking Spiders',
          owner: 'Brian Webb',
          players: [],
          startingSalaryCap: 100,
        },
        {
          logo: 'theHighwaymen',
          name: 'The Highwaymen',
          owner: 'Brad Ray',
          players: [],
          startingSalaryCap: 100,
        },
      ],
    };
  },
  methods: {
    clearClicked() {
      if (window.confirm('Are you sure you want to clear everything?')) {
        window.localStorage.removeItem('Barking Spiders');
        window.localStorage.removeItem('Blitzkrieg');
        window.localStorage.removeItem('Finkle is Einhorn!');
        window.localStorage.removeItem('Endzone Outlaws');
        window.localStorage.removeItem('Detroit HopHeads');
        window.localStorage.removeItem('Fighting Amish');
        window.localStorage.removeItem('The Highwaymen');
        window.localStorage.removeItem('TD Holmesy');
        window.localStorage.removeItem('Rumspringa');
        window.localStorage.removeItem('Rubber Duckies');
        window.localStorage.removeItem('Touch of Class');
        window.localStorage.removeItem('Chevelle SS');
        this.init();
      }
    },

    formatPrice(value) {
      const formatter = new Intl.NumberFormat('en-US', {
        currency: 'USD',
        minimumFractionDigits: 0,
        style: 'currency',
      });
      return formatter.format(value);
    },

    getLogo(name) {
      return require(`./assets/${name}.jpg`);
    },

    getMaxBid(team) {
      return (
        this.getSalaryCapRemaining(team) - this.getPositionsRemaining(team) + 1
      );
    },

    getPositionsRemaining(team) {
      return team.players.filter((player) => !player.name).length;
    },

    getSalaryCapRemaining(team) {
      return team.startingSalaryCap - this.getTotalPlayerSalary(team.players);
    },

    getTotalPlayerSalary(players) {
      return players
        .map((player) => parseInt(player.salary, 10))
        .reduce((prev, curr) => prev + curr, 0);
    },

    init() {
      this.teams = this.teams.map((team) => {
        const localStorage = JSON.parse(window.localStorage.getItem(team.name));

        if (localStorage) {
          team.players = localStorage;
        } else {
          team.players = [
            {
              name: '',
              position: 'QB',
              salary: 0,
            },
            {
              name: '',
              position: 'RB1',
              salary: 0,
            },
            {
              name: '',
              position: 'RB2',
              salary: 0,
            },
            {
              name: '',
              position: 'WR1',
              salary: 0,
            },
            {
              name: '',
              position: 'WR2',
              salary: 0,
            },
            {
              name: '',
              position: 'TE',
              salary: 0,
            },
            {
              name: '',
              position: 'FLEX',
              salary: 0,
            },
            {
              name: '',
              position: 'SUPERFLEX',
              salary: 0,
            },
            {
              name: '',
              position: 'B1',
              salary: 0,
            },
            {
              name: '',
              position: 'B2',
              salary: 0,
            },
            {
              name: '',
              position: 'B3',
              salary: 0,
            },
            {
              name: '',
              position: 'B4',
              salary: 0,
            },
            {
              name: '',
              position: 'B5',
              salary: 0,
            },
            {
              name: '',
              position: 'B6',
              salary: 0,
            },
            {
              name: '',
              position: 'B7',
              salary: 0,
            },
            {
              name: '',
              position: 'B8',
              salary: 0,
            },
          ];
        }
        return team;
      });
    },
  },

  mounted() {
    this.init();
  },

  name: 'App',
};
</script>

<style scoped>
p {
  font-size: 28px;
}

.calculations {
  text-align: right;
}

.division-divider {
  border-bottom: 1px solid white;
}

.team-name {
  margin-left: 8px;
  min-width: 205px;
  text-align: left;
}
</style>
