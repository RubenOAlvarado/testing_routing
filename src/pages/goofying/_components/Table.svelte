<script>
    import Grid from 'gridjs-svelte';
    import { esES } from 'gridjs/l10n';
    import { onMount } from 'svelte';
    import { users } from "../_data";

    const className = {
        container: 'table-responsive',
        table: 'table table-striped table-hover',
        thead: 'table-dark'
    };

    const columns = [
        'Name', 'Username', 'Email', 'Phone'
    ];

    const pagination = {
        enabled: true,
        limit: 5
    };

    const sort = {
        multiColumn: true
    };

    let grid;

    onMount(() => {
        grid.updateConfig({
            data: users,
            columns,
            pagination,
            columns,
            className,
            search: true,
            language: esES
        }).forceRender();
    });
</script>

<Grid bind:instance={grid} data={users} />

<!-- <div class="row justify-content-center">
    <div class="col">
        <div class="table-responsive">
            <table class="table table-striped table-hover">
                <thead class="table-dark">
                    <tr>
                        <th scope="col">Name</th>
                        <th scope="col">Username</th>
                        <th scope="col">Email</th>
                        <th scope="col">Phone</th>
                    </tr>
                </thead>
                <tbody>
                    {#each users as user}
                        <tr>
                            <td> {user.name} </td>
                            <td> {user.username} </td>
                            <td> {user.email} </td>
                            <td> {user.phone} </td>
                        </tr>
                    {/each}
                </tbody>
            </table>
        </div>
    </div>
</div> -->