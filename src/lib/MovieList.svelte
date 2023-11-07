<script lang="ts">
  import { Client, setContextClient, cacheExchange, fetchExchange } from '@urql/svelte';
  import { queryStore, gql, getContextClient } from '@urql/svelte';

  const endpointUrl = 'https://spl-cinema-booking.hasura.app/v1/graphql';
  const token = 'eyJhbGxvd2VkX3NjaGVtYXMiOltdLCJhbGxvd2VkX3RhYmxlcyI6e30sImF0X2hhc2giOiJKSTN1Q3dtZjN1anE3eW9DZHRBOVNnIiwiYXVkIjpbIjI2ZGI2YTZjLWI0NjgtNGJlMi1iZGY2LWEzNGNhZTY2ZjI3ZF9jb25zb2xlIl0sImF1dGhfdGltZSI6MTY5OTM0NDE1MywiY29sbGFib3JhdG9yX3ByaXZpbGVnZXMiOlsiZ3JhcGhxbF9hZG1pbiJdLCJleHAiOjE2OTkzNDc3ODUsImlhdCI6MTY5OTM0NDE4NSwiaXNzIjoiaHR0cHM6Ly9vYXV0aC5wcm8uaGFzdXJhLmlvLyIsImp0aSI6IjQ2Zjc5ZmY0LWZjNjMtNGZmNC05OTMyLWE2YmEwZGEyODZjMSIsIm1ldHJpY3NfZnFkbiI6InVzLXdlc3QtMS1hd3MubWV0cmljcy5wcm8uaGFzdXJhLmlvIiwibm9uY2UiOiIiLCJwcm9qZWN0Ijp7ImlkIjoiMjZkYjZhNmMtYjQ2OC00YmUyLWJkZjYtYTM0Y2FlNjZmMjdkIiwibmFtZSI6InNwbC1jaW5lbWEtYm9va2luZyJ9LCJyYXQiOjE2OTkzNDQxMzgsInNpZCI6ImQ3NjQ0OTk3LTJlOWYtNGY0YS04NDg1LWJhNjI2N2Q3MGRiOCIsInN1YiI6IjhiN2QzMGRhLTgxNzMtNDA5Yy1hYjU2LTNkMGNmNzBiNmI1OSJ9';

  const client = new Client({
    url: endpointUrl,
    exchanges: [cacheExchange, fetchExchange],
    fetchOptions: () => {
      return {
        headers: {
              'content-type': 'application/json',
      'x-hasura-admin-secret':
          'E8qY0zbTjfrdq1xkXSlnU3SWYhYp4wtSzcgKxixH2FUSzn8pxGAZiHRzTLKQLyXm',

        },
      };
    },
  });

  setContextClient(client);

  const movies = queryStore({
    client: getContextClient(),
    query: gql`
      query MyQuery {
        cart_details {
          item_name
          item_count
        }
      }
    `,
  });
</script>

{#if $movies.fetching}
  <p>Loading...</p>
{:else if $movies.error}
  <p>Oh no... {$movies.error.message}</p>
{:else}
<h3> GraphQl + URQL</h3>
  <ul>
    {#each $movies.data.cart_details as item}
      <li> {item.item_name} Count: {item.item_count}  </li>
    {/each}
  </ul>
{/if}