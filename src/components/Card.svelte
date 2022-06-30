<script>
  export let image;
  export let name;
  export let price;
  export let change;
  export let volume;
  export let symbol;
  export let i;
</script>

<tr>
  <td>{i + 1}</td>
  <td>
    <img src={image} alt={name} style="width: 1.5rem;" class="img-fluid me-3" />
    <span>{name}</span>
    <span class="text-muted text-uppercase ms-2">{symbol}</span>
  </td>
  <td>${price.toFixed(2)}</td>
  <td
    class:text-success={change.toFixed() > 0}
    class:text-danger={change.toFixed() < 0.00001}>{change.toFixed(3)}%</td
  >
  <td>${volume.toLocaleString()}</td>
</tr>
