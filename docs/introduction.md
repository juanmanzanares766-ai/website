import flet as ft

def main(page: ft.Page):
    page.title = "Punto de Equilibrio"
    page.scroll = "adaptive"
    page.theme_mode = ft.ThemeMode.LIGHT

    cf_field = ft.TextField(label="Costo Fijo (Cf)", keyboard_type=ft.KeyboardType.NUMBER)
    pv_field = ft.TextField(label="Precio de Venta (Pv)", keyboard_type=ft.KeyboardType.NUMBER)
    cv_field = ft.TextField(label="Costo Variable (Cv)", keyboard_type=ft.KeyboardType.NUMBER)

    mc_result = ft.Text()
    peo_result = ft.Text(color=ft.colors.BLUE)

    def calcular_click(e):
        try:
            cf = float(cf_field.value)
            pv = float(pv_field.value)
            cv = float(cv_field.value)
        except:
            page.snack_bar = ft.SnackBar(ft.Text("⚠️ Ingresa números válidos."))
            page.snack_bar.open = True
            page.update()
            return

        if pv <= cv:
            page.snack_bar = ft.SnackBar(ft.Text("⚠️ Pv debe ser > Cv."))
            page.snack_bar.open = True
            page.update()
            return

        mc = pv - cv
        peo = cf / mc

        mc_result.value = f"Mc: ${mc:.2f}"
        peo_result.value = f"Peo: {peo:.2f} unidades"
        page.update()

    page.add(
        ft.Text("📊 Punto de Equilibrio", size=24, weight="bold"),
        cf_field,
        pv_field,
        cv_field,
        ft.ElevatedButton("Calcular", on_click=calcular_click, width=200),
        mc_result,
        peo_result,
        ft.Text("Hecho con Flet • Python", size=12, italic=True, color=ft.colors.GREY)
    )

ft.app(target=main)
