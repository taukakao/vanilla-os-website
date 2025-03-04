<template>
    <div class="hero hero--big hero--no-margin anim--fadeIn">
        <div class="hero-wrapper container">
            <img class="hero-image no-dark" src="/assets/images/backgrounds/vos-orchid-laptop-frame.svg"
                alt="Vanilla OS 2 Orchid" />
            <img class="hero-image on-dark" src="/assets/images/backgrounds/vos-orchid-laptop-frame-dark.svg"
                alt="Vanilla OS 2 Orchid" />
            <div class="hero-content">
                <div class="hero-imageText">
                    <img src="/assets/images/components/beta-headline.svg" alt="Vanilla OS 2 Beta" />
                </div>
            </div>
        </div>
    </div>
    <div class="container">
        <div class="flexList">
            <div class="flexGrid flexGrid--3 anim--fadeIn">
                <div class="flexGrid-item flexGrid-item--2 card card--type-adv card--type-dev card--has-pattern-work">
                    <div class="card-header">
                        <h4>Disclaimer</h4>
                        <h2>Read before downloading</h2>
                    </div>
                    <div class="card-content">
                        <div class="flexList">
                            <div class="text text--rich">
                                <p>These beta builds are intended for testing and development purposes. Please be aware
                                    that they may contain bugs and may not be stable. We recommend that you do not use
                                    these builds as your daily driver.</p>
                                <p>Beta builds are meant to let you test the latest features and technologies. If you
                                    find a bug, please report it so that we can fix it before the stable release. Thank
                                    you for your help!</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="flexGrid-item card">
                    <div class="card-header">
                        <h4>Changelog</h4>
                        <h2>Discover what's new in Vanilla OS 2 Orchid</h2>
                    </div>
                    <div class="card-content">
                        <div class="flexList">
                            <div class="text text--rich">
                                <p>Orchid is the next generation of Vanilla OS. It is built on top of new
                                    technologies and features to provide you with a more secure and stable system.</p>
                            </div>
                            <router-link
                                :to="{ name: 'article', params: { date: '2024-01-30', slug: 'vanilla-os-2-orchid-beta-is-here' } }"
                                class="btn btn--primary">
                                <span>Learn More</span>
                                <span class=" mdi material-icons-outlined">arrow_forward</span>
                            </router-link>
                        </div>
                    </div>
                </div>
            </div>
            <div class="spacer"></div>
            <div class="text text--rich">
                <h3>Vanilla OS 2 Orchid - Beta builds</h3>
                <p>You must be logged on GitHub to download the beta builds. Once logged in, you will be able to
                    download the chosen build from the artifacts section.</p>
            </div>
            <div class="table">
                <table class="table-content">
                    <thead>
                        <tr>
                            <th>Id</th>
                            <th>Release Date</th>
                            <th>Arch</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr class="table-row" v-for="release in releases" :key="release.Id"
                            :class="release.IsLatest ? 'table-row--highlight' : ''">
                            <td>{{ release.Id }}</td>
                            <td>{{ release.Date }}</td>
                            <td>{{ release.Arch }}</td>
                            <td>
                                <div class="table-row--actions">
                                    <div class="badges" v-if="release.IsLatest">
                                        <span class="badges-item badges-item--green">Latest</span>
                                    </div>
                                    <a class="btn--link btn--inline" target="_blank" :href="release.Url">
                                        <span class="mdi material-icons-outlined">file_download</span>
                                    </a>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="spacer"></div>
            <div class="flexGrid flexGrid--3">
                <div
                    class="flexGrid-item flexGrid-item--2 card card--hz card--type-adv card--type-adv--hz card--type-funnyletter">
                    <div class="card-header">
                        <h4>Be the first to know when stable release is out</h4>
                        <h2>Subscribe to the Newsletter</h2>
                        <div class="btn btn--primary" @click="isNotMailNotChimpOpen = true">
                            <span class="mdi material-icons">email</span>
                            <span>Subscribe Now</span>
                        </div>
                    </div>
                    <div class="card-content">
                        <div class="flexList">
                            <div class="card-image">
                                <img src="/assets/images/components/funnyletter.svg" alt="Subscribe to the Newsletter" />
                            </div>
                        </div>
                    </div>
                </div>
                <router-link class="flexGrid-item card card--clickable card--min card--has-actions flexGrid-item"
                    to="/community">
                    <div class="card-header">
                        <h3><span class="mdi material-icons">record_voice_over</span> Discord Server</h3>
                        <div class="card-actions">
                            <div class="btn btn--link btn--inline btn--med"><span
                                    class="mdi material-icons">arrow_forward</span></div>
                        </div>
                    </div>
                    <div class="card-content">
                        <div class="flexList">
                            <div class="text">
                                <p>Join our Discord server to chat with other users and developers, participate to the
                                    events and get some spoilers.</p>
                            </div>
                        </div>
                    </div>
                </router-link>
            </div>
            <notmail-not-chimp :is-open="isNotMailNotChimpOpen" @close="closeNotMailNotChimp" tagsId="12812945" />
        </div>
    </div>
</template>
  
<script lang="ts">
import { defineComponent } from 'vue';

class Release {
    Id!: string;
    Date!: string;
    Arch!: string;
    Url!: string;
    IsLatest?: boolean;
}

export default defineComponent({
    name: 'download-orchid-beta',
    data() {
        return {
            isNotMailNotChimpOpen: false,
            releases: [] as Release[],
        };
    },
    methods: {
        closeNotMailNotChimp() {
            this.isNotMailNotChimpOpen = false;
        },
    },
    mounted() {
        fetch('https://raw.githubusercontent.com/Vanilla-OS/info/main/betaBuilds.json')
            .then((response) => response.json())
            .then((data) => {
                const _releases = data.map((release: Release) => ({
                    Id: release.Id,
                    Date: new Date(release.Date).toLocaleDateString(),
                    Arch: release.Arch,
                    Url: release.Url,
                }));

                _releases.sort((a: { Date: string | number | Date; }, b: { Date: string | number | Date; }) => new Date(b.Date).getTime() - new Date(a.Date).getTime());
                _releases[0].IsLatest = true;
                this.releases = _releases;
            });
    },
});
</script>
