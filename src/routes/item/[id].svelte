<script context='module'>
    import * as api from 'api'
    export async function preload({params}){
        let {id} = params
        let item = await api.get(`items/${id}`)
        if (item == '404')(
            this.error(404, 'item not Found')
        )
        if (item == '423')(
            this.error(423, 'item not visible')
        )
        return {item}
    }
</script>

<script>
    export let item
    import {
        Row,
        Link,
        Column,
    } from 'carbon-components-svelte'
    import {marked} from 'utils'
    import { stores } from '@sapper/app'
    
    let { session } = stores()

    let description
    if (item.description){
        description = marked(item.description)
    }
</script>

<Row  noGutter>
    <Column lg={2} sm={2} md={2} xlg={2}>
        <div style='width: 111px; height: 111px;'>
            <img width=100% height=100% alt='itemImage' src={item.image}>
        </div>
    </Column>
    <Column lg={4} sm={4} md={4} xlg={4}>
        {#if item.name}
            <h2>{item.name}</h2>
        {/if}
        <p>{item.itype}</p>
        {#if item.address}
            <p>{item.address}</p>
        {/if}
        {#if item.website}
            <Link href={item.website}>{item.website}</Link>
        {/if}
        {#if item.email}
            <p>{item.email}</p>
        {/if}
        {#if item.phone}
            <p>{item.phone}</p>
        {/if}
        <!-- <Link href='' on:click={open}>Gallery</Link> -->
        <Link href='{item.user}'>User</Link>
        {#if $session.user.username == item.user}
            <Link href='edit/{item.id}'>Edit</Link>
        {/if}
    </Column>
</Row>

{#if description}
    <Row noGutter>
        <Column lg={2} sm={2} md={2} xlg={2}>
            <p>{@html description}</p>
        </Column>
    </Row>
{/if}