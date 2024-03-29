<template>
  <q-page>
    <section class="row justify-evenly container" id="home">
      <div class="col-xs-12 col-sm-6 col-md-6">
        <h1
          class="text-primary text-bold"
          :class="$q.screen.lt.sm ? 'text-h4' : 'text-h2'"
        >
          ¡Obtén El Apoyo Financiero Que Necesitas Hoy Mismo!
        </h1>
        <h2
          class="text-secondary"
          :class="$q.screen.lt.sm ? 'text-h6' : 'text-h5'"
        >
          Bienvenido a RapiPlata, tu aliado financiero confiable. Estamos aquí
          para ayudarte a alcanzar tus metas financieras.
        </h2>
        <div class="row justify-center q-mt-xl">
          <q-btn
            color="primary"
            label="Solicitar cupo de crédito"
            no-caps
            rounded
            unelevated
            class="paddig_button"
            @click="scrollTo('simulador')"
          />
        </div>
      </div>
      <div class="col-xs-12 col-sm-6 col-md-6" v-if="$q.screen.gt.xs">
        <q-img
          src="/img/ahorro.png"
          alt="Alcancia"
          fit="contain"
          style="height: 450px"
        />
      </div>
    </section>

    <section
      class="section_simulator row container"
      :id="$q.screen.gt.sm ? 'simulador' : ''"
    >
      <div class="col-xs-12 col-sm-12 col-md-6 q-px-md">
        <h3 class="text-h6" v-if="$q.screen.gt.xs">
          Tenemos la mejor herramienta perfecta para planificar tu futuro
          financiero con confianza. ¿Te preguntas cuánto podrías solicitar o
          cuál sería la mejor opción de crédito para tus necesidades? Con
          nuestro calculador, obtendrás respuestas rápidas y precisas.
        </h3>

        <div class="q-mt-xl">
          <p class="text-body1 text-bold">Ventajas del simulador:</p>
          <ol class="advantages">
            <li>
              Ajusta el monto del crédito y el plazo de pago según tus
              necesidades únicas. Nuestro calculador te permite personalizar
              cada detalle para obtener resultados específicos.
            </li>
            <li>
              Visualiza de manera clara y detallada los pagos mensuales,
              incluyendo la tasa de interés y cualquier cargo adicional. Así
              podrás tomar decisiones informadas sobre tu capacidad de pago.
            </li>
          </ol>
        </div>
      </div>
      <div
        class="col-xs-12 col-sm-12 col-md-6 row"
        :class="
          $q.screen.lt.md ? 'q-px-md justify-center' : 'q-mt-xl justify-end'
        "
        :id="$q.screen.lt.md ? 'simulador' : ''"
      >
        <q-card
          class="card_simulator q-pb-md"
          style="max-width: 90vw; width: 450px"
        >
          <q-card-section>
            <div class="text-h6 text-center text-secondary q-my-md">
              Calculadora de crédito
            </div>
            <q-separator />
            <q-list dense>
              <q-item class="text-secondary text-bold">
                <q-item-section> Cuanto dinero necesitas? </q-item-section>
                <q-item-section avatar class="text-h6">
                  $ {{ new Intl.NumberFormat().format(request.monto) }}
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label>
                    <q-slider
                      v-model="request.monto"
                      :min="min"
                      :max="max"
                      :step="50000"
                      color="primary"
                      track-size="10px"
                      thumb-size="35px"
                    />
                  </q-item-label>
                  <q-item-label class="row">
                    <span> $ {{ new Intl.NumberFormat().format(min) }} </span>
                    <q-space />
                    <span> $ {{ new Intl.NumberFormat().format(max) }} </span>
                  </q-item-label>
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="text-secondary text-bold">
                    A cuantas cuotas quieres pagarlo?
                  </q-item-label>
                  <div class="row justify-center" v-if="$q.screen.gt.xs">
                    <template v-for="(option, index) in options" :key="index">
                      <q-radio
                        :val="option.value"
                        v-model="request.plazo"
                        size="60px"
                        :label="option.label"
                      />
                    </template>
                  </div>
                  <q-option-group
                    v-else
                    v-model="request.plazo"
                    :options="options"
                  />
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="text-secondary text-bold">
                    Cada cuanto quieres pagarlo?
                  </q-item-label>
                  <div class="row justify-center" v-if="$q.screen.gt.xs">
                    <template
                      v-for="(option, index) in optionPeriodo"
                      :key="index"
                    >
                      <q-radio
                        :val="option.value"
                        v-model="periodo"
                        size="60px"
                        :label="option.label"
                      />
                    </template>
                  </div>
                  <q-option-group
                    v-else
                    v-model="periodo"
                    :options="optionPeriodo"
                  />
                </q-item-section>
              </q-item>
              <q-item class="text-secondary text-bold">
                <q-item-section> Cantidad a solicitar </q-item-section>
                <q-item-section avatar>
                  $ {{ new Intl.NumberFormat().format(request.monto) }}
                </q-item-section>
              </q-item>
              <q-item class="text-secondary text-bold">
                <q-item-section> Administración </q-item-section>
                <q-item-section avatar>
                  $ {{ new Intl.NumberFormat().format(cuotaAdministracion) }}
                </q-item-section>
              </q-item>
              <q-item class="text-secondary text-bold">
                <q-item-section> Interés </q-item-section>
                <q-item-section avatar>
                  %
                  {{ periodo == 15 ? +interestRate / 2 : interestRate }}
                </q-item-section>
              </q-item>
            </q-list>

            <q-separator />

            <q-list dense>
              <q-item class="text-secondary text-bold">
                <q-item-section> Fecha </q-item-section>
                <q-item-section avatar> Cuota </q-item-section>
              </q-item>
              <q-item v-for="(k, i) in amortizacion" :key="i">
                <q-item-section>{{ k.fechaPago }}</q-item-section>
                <q-item-section avatar>
                  $ {{ new Intl.NumberFormat().format(k.cuota) }}
                </q-item-section>
              </q-item>
            </q-list>

            <q-separator />
            <q-list dense>
              <q-item class="text-secondary text-bold">
                <q-item-section> Total a pagar </q-item-section>
                <q-item-section avatar>
                  $ {{ new Intl.NumberFormat().format(totalPagar) }}
                </q-item-section>
              </q-item>
            </q-list>
          </q-card-section>
          <q-card-actions align="center">
            <q-btn
              color="primary"
              no-caps
              rounded
              unelevated
              @click="dialogForm = true"
              label="Solicitar cupo de crédito"
              class="paddig_button"
            />
          </q-card-actions>
        </q-card>
      </div>
    </section>

    <!-- Dialog para el formulario -->
    <q-dialog v-model="dialogForm">
      <q-card style="width: 450px; max-width: 90vw" class="q-pa-md">
        <q-bar class="bg-white">
          <q-space />
          <div class="text-center text-secondary text-h5 text-bold">
            Formulario de solicitud
          </div>
          <q-space />
        </q-bar>
        <q-card-section>
          <q-form @submit="onSubmit" class="row">
            <div class="col-xs-12 col-md-6 q-pa-xs">
              <q-input
                v-model="request.nombres"
                type="text"
                label="Nombres"
                outlined
                size="35px"
                :rules="[(val) => !!val || 'Campo requerido']"
                hide-bottom-space
              />
            </div>
            <div class="col-xs-12 col-md-6 q-pa-xs">
              <q-input
                v-model="request.apellidos"
                type="text"
                label="Apellidos"
                outlined
                size="35px"
                :rules="[(val) => !!val || 'Campo requerido']"
                hide-bottom-space
              />
            </div>
            <div class="col-xs-12 q-pa-xs">
              <q-input
                v-model="request.email"
                type="email"
                label="Email"
                outlined
                size="35px"
                :rules="[(val) => !!val || 'Campo requerido']"
                hide-bottom-space
              />
            </div>
            <div class="col-xs-12 q-pa-xs">
              <q-input
                v-model="request.celular"
                label="Celular"
                outlined
                size="35px"
                :rules="[(val) => !!val || 'Campo requerido']"
                mask="##########"
                hide-bottom-space
              />
            </div>
            <div class="col-xs-12 q-pa-xs">
              <q-select
                v-model="request.fkIdTipoDocumento"
                :options="optionsDocuments"
                label="Tipo documento"
                map-options
                emit-value
                option-value="idTipoDocumento"
                option-label="nombreTipoDocumento"
                outlined
                size="35px"
              />
            </div>
            <div class="col-xs-12 q-pa-xs">
              <q-input
                v-model="request.documentoPersona"
                label="Número de documento"
                outlined
                size="35px"
                :rules="[(val) => !!val || 'Campo requerido']"
                hide-bottom-space
                mask="###############"
              />
            </div>
            <div class="col-xs-12 col-md-6 q-pa-xs">
              <q-field outlined label="Monto solicitado" stack-label>
                <template v-slot:control>
                  <div class="self-center full-width no-outline" tabindex="0">
                    {{ new Intl.NumberFormat().format(request.monto) }}
                  </div>
                </template>
              </q-field>
            </div>
            <div class="col-xs-12 col-md-6 q-pa-xs">
              <q-field outlined label="Plazo" stack-label>
                <template v-slot:control>
                  <div class="self-center full-width no-outline" tabindex="0">
                    {{ new Intl.NumberFormat().format(request.plazo) }}
                  </div>
                </template>
              </q-field>
            </div>
            <div class="col-xs-12 q-pa-xs" v-if="errorNotFound">
              <q-banner inline-actions rounded class="bg-red text-white">
                {{ errorNotFound }}

                <template v-slot:action>
                  <q-btn
                    flat
                    label="Cerrar"
                    no-caps
                    @click="errorNotFound = ''"
                  />
                </template>
              </q-banner>
            </div>
            <div class="col-xs-12 q-pa-xs" v-if="errorMatchData">
              <q-banner inline-actions rounded class="bg-orange text-white">
                {{ errorMatchData }}

                <template v-slot:action>
                  <q-btn
                    flat
                    label="Cerrar"
                    no-caps
                    @click="errorMatchData = ''"
                  />
                </template>
              </q-banner>
            </div>
            <div class="col-xs-12 row justify-center q-pa-xs q-mt-md">
              <q-btn
                label="Solicitar cupo de crédito"
                rounded
                unelevated
                type="submit"
                color="primary"
                no-caps
                class="paddig_button"
              />
            </div>
            <div class="col-xs-12 row justify-center q-pa-xs">
              <q-btn
                label="Solicitar nuevo monto"
                rounded
                unelevated
                color="negative"
                flat
                no-caps
                @click="closeDialog"
                class="paddig_button"
              />
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>

    <!-- Pasos para obtener el credito -->
    <section class="section_steps row container" id="steps">
      <div
        class="col-xs-12 q-my-xl"
        :style="$q.screen.lt.md ? 'margin-top: 170px; margin-bottom: 0px;' : ''"
      >
        <h4 class="text-h5 text-center text-bold text-white">
          Pide tu crédito 100% en línea en cinco pasos.
        </h4>
      </div>
      <div class="col-xs-12 row text-white">
        <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
          <p class="text-h2 text-bold">01</p>
          <p class="subtitle">Ser empleado</p>
          <p>Verifica que eres colaborador de nuestra compañia</p>
        </div>
        <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
          <p class="text-h2 text-bold">02</p>
          <p class="subtitle">Calcula tu crédito</p>
          <p>
            Escoge el valor, el plazo de tu crédito y valida que se acomode a
            tus necesidades
          </p>
        </div>
        <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
          <p class="text-h2 text-bold">03</p>
          <p class="subtitle">Realiza tu solicitud</p>
          <p>Llena el formulario con la información requerida</p>
        </div>
        <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
          <p class="text-h2 text-bold">04</p>
          <p class="subtitle">Espera nuestra aprobación</p>
          <p>Realizaremos el estudio y te notificamos cuando sea aprobado</p>
        </div>
        <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
          <p class="text-h2 text-bold">05</p>
          <p class="subtitle">Recibe tu dinero</p>
          <p>Desembolsamos tu dinero a tu cuenta de nómia</p>
        </div>
      </div>
    </section>

    <footer
      class="footer row"
      :class="$q.screen.lt.md ? 'q-pt-md' : 'container'"
    >
      <div
        class="q-px-md"
        :class="$q.screen.lt.md ? 'col-xs-12 q-pb-md' : 'col'"
      >
        <q-img src="/img/logo.png" fit="contain" style="height: 35px" />
      </div>
      <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
        <p class="subtitle">Contacto</p>
        <ul>
          <li><q-icon name="mail" /> email@gmail.com</li>
          <li><q-icon name="phone" /> 60132342</li>
          <li><q-icon name="smartphone" /> 3123213112</li>
        </ul>
      </div>
      <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
        <p class="subtitle">Nosotros</p>
        <ul>
          <li>RapiPlata</li>
          <li>Nit. 12131231-2</li>
          <li>Cl 12 #12-2</li>
          <li>gerencia@gmail.com</li>
        </ul>
      </div>
      <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
        <p class="subtitle">Legales</p>
        <ul>
          <li>Términos y condiciones</li>
          <li>Política de privacidad</li>
        </ul>
      </div>
      <div class="q-px-md" :class="$q.screen.lt.md ? 'col-xs-6' : 'col'">
        <p class="subtitle">Redes</p>
        <ul>
          <li><q-icon name="fa-brands fa-facebook" /> empleo.com</li>
          <li><q-icon name="fa-brands fa-instagram" /> @empleo.com</li>
        </ul>
      </div>
    </footer>
    <q-separator />
    <div class="row">
      <div class="col-xs-12 text-center q-pt-md">
        <p>&copy; {{ date.formatDate(Date.now(), 'YYYY') }} RapiPlata.</p>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
import { date, LocalStorage, useQuasar } from 'quasar';
import { api } from '../boot/axios';
import {
  Cuota,
  ResDocuments,
  ResPerson,
  TipoDocumento,
} from 'src/components/models';
import { scrollTo } from '../composables/scroll';

const $q = useQuasar();

const min = ref(50000);
const max = ref(10000000);
const options = [
  { label: '2 Cuotas', value: 2 },
  { label: '3 Cuotas', value: 3 },
  { label: '4 Cuotas', value: 4 },
];
const optionPeriodo = [
  { label: 'Quincenal', value: 15 },
  { label: 'Mensual', value: 30 },
];
const dialogForm = ref(false);
const periodo = ref(15);
const request = ref({
  monto: 50000,
  plazo: 4,
  idProducto: 1,
  documentoPersona: '',
  nombres: '',
  apellidos: '',
  email: '',
  fkIdTipoDocumento: 1,
  celular: '',
});
const interestRate = ref<number>(0.03);
const optionsDocuments = ref<TipoDocumento[]>([]);
const amortizacion = ref<Cuota[]>([]);
const totalPagar = ref(0);
const errorMatchData = ref('');
const errorNotFound = ref('');
const cuotaAdministracion = ref(0);

const onSubmit = async () => {
  $q.loading.show({
    message: 'Enviado solicitud, por favor espere...',
  });
  try {
    errorMatchData.value = '';
    errorNotFound.value = '';
    const {
      data: { person },
    } = await api.get<ResPerson>(`/person/${request.value.documentoPersona}`);

    if (
      person.email !== request.value.email ||
      person.celular !== Number(request.value.celular)
    ) {
      errorMatchData.value =
        'Los datos de contacto no coinciden, debe actualizar datos, comuniquese con el adminitrador del sistema';

      return;
    }
    await api.post('/solicitude', request.value);

    request.value = {
      monto: 100000,
      plazo: 2,
      idProducto: 86,
      documentoPersona: '',
      nombres: '',
      apellidos: '',
      email: '',
      fkIdTipoDocumento: 1,
      celular: '',
    };

    $q.notify({
      type: 'positive',
      message: 'Solicitud enviada con éxito',
      position: 'bottom-right',
    });
    dialogForm.value = false;
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
  } catch (error: any) {
    if (error.response.status == 404) {
      errorNotFound.value =
        'Lo sentimos, no hemos encontrado datos asociados a los ingresados';
      return;
    }

    errorNotFound.value = error.message;
  } finally {
    $q.loading.hide();
  }
};

const getData = async () => {
  try {
    // Login en la api
    const { headers } = await api.post('login', {
      usuario: '1006721563',
      password: '1006721563',
    });

    api.defaults.headers.common['x-access-token'] = headers['auth-token'];

    LocalStorage.set('token', headers['auth-token']);

    // Tipos de documentos
    const {
      data: { types },
    } = await api.get<ResDocuments>('/document_types');

    optionsDocuments.value = [...types];
  } catch (error) {
    console.log(error);
  }
};

const calculateAmortizacion = () => {
  amortizacion.value.length = 0;

  const timeStamp = Date.now();

  const firtsCuot = date.addToDate(
    timeStamp,
    periodo.value == 30 ? { months: 1 } : { days: periodo.value }
  );
  const formattedString = date.formatDate(firtsCuot, 'YYYY-MM-DD');

  // calculamos la cuota
  const cuota = Math.round(calculateCuota());

  // Calcula la primera fecha de pago
  cuotaAdministracion.value = request.value.monto * 0.01;

  amortizacion.value.push({
    fechaPago: formattedString,
    cuota: cuota + cuotaAdministracion.value,
  });
  totalPagar.value = cuota + cuotaAdministracion.value;

  // Calcula el resto de fechas de pago
  for (let index = 1; index < request.value.plazo; index++) {
    const otherTimeStap = new Date(
      amortizacion.value[amortizacion.value.length - 1].fechaPago
    );
    const newDate = date.addToDate(
      otherTimeStap,
      periodo.value == 30 ? { months: 1 } : { days: periodo.value }
    );
    amortizacion.value.push({
      fechaPago: date.formatDate(newDate, 'YYYY-MM-DD'),
      cuota,
    });

    totalPagar.value = totalPagar.value + cuota;
  }
};

const calculateCuota = () => {
  const interes =
    periodo.value == 15 ? interestRate.value / 2 : interestRate.value;

  const numeroCuotas = request.value.plazo;

  const monto = Number(request.value.monto);

  const cuota =
    (monto * interes * Math.pow(1 + interes, numeroCuotas)) /
    (Math.pow(1 + interes, numeroCuotas) - 1);

  return cuota;
};

const closeDialog = () => {
  dialogForm.value = false;
  setTimeout(() => {
    scrollTo('simulador');
  }, 500);
};

watch(
  () => request.value.plazo,
  () => {
    calculateAmortizacion();
  }
);

watch(
  () => request.value.monto,
  () => {
    calculateAmortizacion();
  }
);

watch(periodo, () => {
  calculateAmortizacion();
});

onMounted(async () => {
  await getData();
  calculateAmortizacion();
  calculateCuota();
});
</script>
<style lang="scss" scoped>
.subtitle {
  font-size: 18px;
  font-weight: bold;
}
</style>
