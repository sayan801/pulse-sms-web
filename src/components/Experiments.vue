<template>
    <div id="settings">
        <div id="experiments-list" v-mdl class="page-content">
            <h4>{{ $t('experiments.disclaimer') }}</h4>
            <div class="mdl-color-text--grey-600">
                {{ $t('experiments.explanatory_intro') }}
            </div>
            <br>
            <h4>{{ $t('settings.theme') }}</h4>
            <div class="label-item">
                <label for="larger_app_bar" class="mdl-switch mdl-js-switch mdl-js-ripple-effect mdl-js-ripple-effect--ignore-events">
                    <input id="larger_app_bar" v-model="larger_app_bar" class="mdl-switch__input" type="checkbox">
                    <span class="mdl-switch__label mdl-color-text--grey-900">
                        {{ $t('experiments.larger_app_bar') }}
                    </span>
                </label>
            </div>
            <div class="label-item">
                <label for="unread_count_in_sidebar" class="mdl-switch mdl-js-switch mdl-js-ripple-effect mdl-js-ripple-effect--ignore-events">
                    <input id="unread_count_in_sidebar" v-model="unread_count_in_sidebar" class="mdl-switch__input" type="checkbox">
                    <span class="mdl-switch__label mdl-color-text--grey-900">
                        {{ $t('experiments.unread_count_in_sidebar') }}
                    </span>
                </label>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'Experiments',

    data () {
        return {
            title: "Experiments",
            larger_app_bar: this.$store.state.larger_app_bar,
            unread_count_in_sidebar: this.$store.state.unread_count_in_sidebar,
        };
    },
    watch: {
        'larger_app_bar' () {
            const body = document.querySelector('body'); // Select body
            const LARGER_APP_BAR = "larger_app_bar";

            this.$store.commit('larger_app_bar', this.larger_app_bar);

            // Add class
            if (this.larger_app_bar && !body.className.includes(LARGER_APP_BAR))
                body.className += ` ${LARGER_APP_BAR} `;
            // Remove class
            else if (!this.larger_app_bar && body.className.includes(LARGER_APP_BAR))
                body.className = body.className.replace(` ${LARGER_APP_BAR} `, "");

        },

        'unread_count_in_sidebar' () {
            this.$store.commit('unread_count_in_sidebar', this.unread_count_in_sidebar);
        }
    },

    mounted () {
        this.$store.commit('title', this.title);
        this.$store.state.msgbus.$on('refresh-btn', this.refreshExperiments);
    },

    beforeDestroy () {
        this.$store.state.msgbus.$off('refresh-btn', this.refreshExperiments);
    },

    methods: {
        refreshExperiments () {

        },
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
    @import "../assets/scss/_vars.scss";

    .label-item {
        padding-left: 15px;
    }

    .item, .click-item {
        position: relative;
        width: 100%;
        padding: 16px;
        line-height: 18px;
    }

    .item:hover, .click-item:hover {
          background: #E0E0E0;
    }

    .click-item:hover {
        cursor: pointer;
    }

    body.dark {
        .item:hover, .click-item:hover {
            background: #202020;
        }
    }

</style>
