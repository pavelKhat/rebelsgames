<template>
    <div>
        <b-navbar fixed="top" toggleable="lg" type="dark" variant="dark">
            <b-navbar-brand href="#">
                <img id="rg-logo" src="/images/Logo-Rebels-64x64.png" alt="" class="d-inline-block img-thumbnail"> Rebels-Games
            </b-navbar-brand>

            <b-navbar-toggle target="nav_collapse" />

            <b-collapse is-nav id="nav_collapse">
                <!-- Right aligned nav items -->
                <b-navbar-nav class="ml-auto">

                    <b-nav-item :to="uri(el.to)" v-for="(el, index) in menu[currentLocale]" :key="index">
                        {{ el.name }}
                    </b-nav-item>

                    <b-nav-item href="https://steamcommunity.com/openid/login?openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0&openid.mode=checkid_setup&openid.return_to=https%3A%2F%2Fspace-engineers.com%2Fserver%2F112111%2Fvote%2Faction%2F&openid.realm=https%3A%2F%2Fspace-engineers.com&openid.ns.sreg=http%3A%2F%2Fopenid.net%2Fextensions%2Fsreg%2F1.1&openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select" target="_blank">Vote</b-nav-item>

                    <b-nav-item-dropdown :text="dropdownLang[currentLocale]" right>
                        <b-dropdown-item 
                            v-for="locale in locales"
                            :key="locale.code"
                            :to="currentPath(locale.code)"
                        >
                            <img :src="icons[locale.code]" class="country-icon" />
                            <span class="locale-name">{{locale.name}}</span>
                        </b-dropdown-item>
                    </b-nav-item-dropdown>

                    <b-nav-item-dropdown right>
                    <!-- Using button-content slot -->
                    <template slot="button-content"><em>User</em></template>
                        <b-dropdown-item href="#">Profile</b-dropdown-item>
                        <b-dropdown-item href="#">Signout</b-dropdown-item>
                    </b-nav-item-dropdown>

                </b-navbar-nav>
            </b-collapse>
        </b-navbar>
    </div>
</template>

<script>

import enIcon from 'svg-country-flags/svg/gb.svg'
import plIcon from 'svg-country-flags/svg/pl.svg'
import ruIcon from 'svg-country-flags/svg/ru.svg'
import { locales } from '../plugins/i18n/i18n.js'


import i18n from '../plugins/i18n';

export default {
    data() {
        return {
            menu: {
                en: [
                    { name: 'News', to: 'news' },
                    { name: 'Space Engineers', to: 'spaceengineers' },
                    { name: 'Donate', to: 'donate' },
                    { name: 'Rebels Crew', to: 'rebelscrew' },
                    { name: 'FAQ', to: 'faq' },
                ],
                pl: [
                    { name: 'Aktualności', to: 'news' },
                    { name: 'Kosmiczni Inżynierowie', to: 'spaceengineers' },
                    { name: 'Dotacja', to: 'donate' },
                    { name: 'Załoga Rebels', to: 'rebelscrew' },
                    { name: 'FAQ', to: 'faq' },
                ],
                ru: [
                    { name: 'весть', to: 'news'  },
                    { name: 'Космические инженеры', to: 'spaceengineers' },
                    { name: 'дарить', to: 'donate' },
                    { name: 'экипаж Rebels', to: 'rebelscrew' },
                    { name: 'FAQ', to: 'faq' },
                ],
            },
            dropdownLang: {
                en: 'Lang',
                pl: 'Język',
                ru: 'язык'    
            },
            icons: {
                en: enIcon,
                pl: plIcon,
                ru: ruIcon,
            },
            locales
        }
    },
    methods: {
        uri(to) {
            const locale = this.$route.params.locale 
            if (to === '/') return `/${locale}`
            return `/${locale}/${to.replace(/^\/|\/$/g, '')}`
        },
        currentPath(lang) {
            const path = this.$route.path;
            return `/${lang}/${path.substr(4)}`;
        },
    },
    computed: {
        currentLocale() {
            i18n.locale = this.$route.params.locale;
            return this.$route.params.locale
        },
    },
}
</script>

<style scoped>
    .navbar {
        padding: 0 1rem 0 1rem !important;
    }

    #rg-logo {
        background: 0 0;
        border: 0;
        height: 44px !important;
        width: 44px !important;
    }

    .country-icon {
        width: 20px;
        height: auto;
        display: inline-block;
        vertical-align: baseline;
        border: 1px solid #dee2e6;
        box-shadow: 0px 1px 3px rgba(24, 29, 38, 0.1);
    }
 
    .country-icon.as-toggle {
        margin-top: 5px;
    }
 
    .locale-name {
        display: inline-block;
        vertical-align: baseline;
    }
</style>