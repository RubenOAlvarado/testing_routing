<script>
    import { users } from "../_data";
</script>

<div class="table-responsive">
    <table class="table table-striped table-hover">
        <thead>
            <tr class="table-primary">
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