<template>
	<div id="submissions"
	     class="home-submissions">
		<div class="flex-space">
			<h2>
				Latest Registered Users:
			</h2>

			<div>
				<el-button type="info"
				           round
				           :loading="users.loading"
				           size="mini"
				           class="margin-left-half"
				           @click="getUsers">
					Refresh
				</el-button>
			</div>
		</div>

		<el-table :data="users.items"
		          stripe
		          size="medium">
			<el-table-column label="Username">
				<template slot-scope="scope">
					<img :src="scope.row.avatar"
					     :alt="scope.row.avatar"
					     height="20"
					     class="circle margin-right-half">

					<span v-text="str_limit(scope.row.username, 15)"></span>
				</template>
			</el-table-column>

			<el-table-column label="Confirmed Email">
				<template slot-scope="scope">
					<el-tag type="info"
					        v-if="scope.row.verified_email"
					        size="mini">
						Yes
					</el-tag>
				</template>
			</el-table-column>

			<el-table-column label="Joined At">
				<template slot-scope="scope">
					<el-tag type="info"
					        size="mini">
						{{ date(scope.row.created_at) }}
					</el-tag>
				</template>
			</el-table-column>

			<el-table-column fixed="right"
			                 label="Operations"
			                 width="180">
				<template slot-scope="scope">
					<el-button type="text"
					           size="small">Details</el-button>

					<el-button type="text"
					           size="small">Ban</el-button>
					
					<el-button type="text"
					           size="small">Message</el-button>
				</template>
			</el-table-column>
		</el-table>
	</div>
</template>

<script>
import Helpers from '../mixins/Helpers';

export default {
    mixins: [Helpers],

    created() {
        this.getUsers();
    },

    data() {
        return {
            users: {
                items: [],
                loading: false
            }
        };
    },

    methods: {
        getUsers() {
            this.users.items = [];
            this.users.loading = true;

            axios
                .get('/admin/users')
                .then(response => {
                    this.users.items = response.data.data;
                    this.users.loading = false;
                })
                .catch(() => {
                    this.users.loading = false;
                });
        },

        date(date) {
            return moment(date)
                .utc(moment().format('Z'))
                .fromNow();
        }
    }
};
</script>
