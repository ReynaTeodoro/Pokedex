<template>
  <v-treeview :items="items" dense class="text-capitalize">
    <template v-slot:prepend="{ item }">
      <v-avatar v-if="!item.children" size="25">
        <img :src="require(`@/assets/moves/egg.png${''}`)" v-if="item.icon === 'egg'" />
        <img :src="require(`@/assets/moves/mt.png${''}`)" v-if="item.icon === 'tm'" />
        <img :src="require(`@/assets/moves/tutor.png${''}`)" v-if="item.icon === 'tutor'" />
        <img :src="require(`@/assets/moves/lvl-up.png${''}`)" v-if="item.icon === 'level'" />
      </v-avatar>
    </template>
    <template v-slot:append="{ item }">
      <v-card-text v-if="!item.children && item.icon === 'level'">{{ item.level }}</v-card-text>
    </template></v-treeview
  >
</template>

<script>
export default {
  props: ['moves'],
  computed: {
    items() {
      const items = this.moves.reduce(
        (acc, el) => {
          let move = {};
          switch (el.version_group_details[0].move_learn_method.name) {
            case 'egg':
              move = {
                name: el.move.name.replace(/-/g, ' '),
                icon: 'egg',
              };
              acc[1].children.push(move);
              break;
            case 'level-up':
              move = {
                name: el.move.name.replace(/-/g, ' '),
                icon: 'level',
                level: el.version_group_details[0].level_learned_at,
              };
              acc[0].children.push(move);
              break;
            case 'tutor':
              move = {
                name: el.move.name.replace(/-/g, ' '),
                icon: 'tutor',
              };
              acc[2].children.push(move);
              break;
            case 'machine':
              move = {
                name: el.move.name.replace(/-/g, ' '),
                icon: 'tm',
              };
              acc[3].children.push(move);
              break;
            default:
              break;
          }
          return acc;
        },
        [
          { id: 1, name: 'Level-Up', children: [] },
          { id: 2, name: 'Egg', children: [] },
          { id: 3, name: 'Tutor', children: [] },
          { id: 4, name: 'TM', children: [] },
        ],
      );
      items[0].children.sort((a, b) => a.level - b.level);
      return items;
    },
  },
};
</script>
