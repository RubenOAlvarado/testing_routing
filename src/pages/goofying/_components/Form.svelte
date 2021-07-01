<script>
    import { createForm } from 'felte';
    import { validator } from '@felte/validator-yup';
    import * as yup from 'yup';
    import reporterDom from '@felte/reporter-dom';
    import Photos from './Photos.svelte';

    const schema = yup.object({
        profile: yup.object({
            name: yup.string().required('El nombre es requerido'),
            email: yup.string().email('Debe ser un email').required('El email es requerido')
        }),
        details: yup.object({
            type: yup.string().required('El tipo es requerido')
        })
    });

    const { form } = createForm({
        extend: [validator, reporterDom()],
        validateSchema: schema,
        onSubmit: (values) => console.log(values)
    });
</script>
<div class="row mb-3 justify-content-center">
    <div class="col">
        <form use:form>
            <fieldset name="profile">
                <legend>Perfil</legend>
                <div class="row mb-3">
                    <div class="col">
                        <label for="name" class="form-label"><strong>Nombre:</strong></label>
                        <input type="text" class="form-control" name="name" placeholder="Nombre" aria-describedby="name-validation" />
                        <div id="name-validation" data-felte-reporter-dom-for="name" class="alert-danger" aria-live="polite" />
                    </div>
                    <div class="col">
                        <label for="email" class="form-label"><strong>Email:</strong></label>
                        <input type="email" class="form-control" name="email" placeholder="Correo Electrónico" aria-describedby="email-validation" />
                        <div id="email-validation" data-felte-reporter-dom-for="email" class="alert-danger" aria-live="polite" />
                    </div>
                    <div class="col">
                        <label for="photo">Foto:</label>
                        <Photos  />
                    </div>
                </div>
            </fieldset>
            <fieldset name="details">
                <legend>Detalle:</legend>
                <label for="type" class="form-label"><strong>Tipo</strong></label>
                <select name="type" id="type" class="form-select" aria-label="Default select example" aria-describedby="type-validation">
                    <option value="">Por favor elije uno</option>
                    <option value="regular">Regular</option>
                    <option value="special">Especial</option>
                </select>
                <div id="type-validation" data-felte-reporter-dom-for="type" class="alert-danger" aria-live="polite" />
            </fieldset>
            <input type="submit" class="btn btn-primary" value="Guardar" />
            <button type="button" class="btn btn-danger">Cancelar</button>
        </form>
    </div>
</div>