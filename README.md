# 🏨 App Hotel (Com Tela 'Sobre') — .NET MAUI

Atividade da **Agenda 13** — Desenvolvimento de Sistemas II (DS2)  
**Centro Paula Souza - ETEC**  
**Aluno:** Donizete dos Santos  
**Ano de Desenvolvimento:** 2026  
**Professor Mediador:** Tadeu Fernando Piran  

---

## 📌 Descrição do Projeto

Aplicação móvel desenvolvida em **.NET MAUI (C# e XAML)** para cálculo e reserva de acomodações de hotel. 
O projeto inclui personalização visual, componentes avançados (`Picker`, `Stepper`, `DatePicker`), passagem de dados por `BindingContext` e navegação para a tela **Sobre**.

---

## 🛠️ Recursos e Componentes Aplicados

- **`Picker`:** Seleção do tipo de suíte (Suíte Luxo, Executive, Crise).
- **`Stepper`:** Controle incremental para ajuste da quantidade de adultos e crianças.
- **`DatePicker`:** Seleção de datas de Check-In e Check-Out formatadas em `dd/MM/yyyy`.
- **`BindingContext`:** Vinculação e transferência do objeto de modelo `Hospedagem` entre telas.
- **Tela `SobrePage`:** Apresentação das informações do desenvolvedor (Donizete dos Santos, 2026).

---

## 📹 Vídeo de Demonstração e Explicativo do Código

🎥 **Assista ao vídeo explicativo com o app rodando:**  
`[COLE_AQUI_O_LINK_DO_SEU_VIDEO_DO_YOUTUBE]`

---

## 💻 Código-Fonte da Aplicação

### Modelo de Dados (`Hospedagem.cs`)
```csharp
using System;

namespace MauiAppHotel.Models
{
    public class Hospedagem
    {
        public string QuartoSelecionado { get; set; }
        public int QuantidadeAdultos { get; set; }
        public int QuantidadeCriancas { get; set; }
        public DateTime DataCheckIn { get; set; }
        public DateTime DataCheckOut { get; set; }

        public int DiasHospedagem => (DataCheckOut - DataCheckIn).Days;

        public double ValorTotal
        {
            get
            {
                double diaria = QuartoSelecionado switch
                {
                    "Suíte Luxo" => 250.0,
                    "Suíte Executive" => 180.0,
                    _ => 100.0
                };

                double totalAdultos = QuantidadeAdultos * diaria * DiasHospedagem;
                double totalCriancas = QuantidadeCriancas * (diaria * 0.5) * DiasHospedagem;

                return totalAdultos + totalCriancas;
            }
        }
    }
}

<?xml version="1.0" encoding="utf-8" ?>
<ContentPage Title="Reserva de Hospedagem" x:Class="MauiAppHotel.Views.ContratacaoHospedagemPage" xmlns="[http://schemas.microsoft.com/dotnet/2021/maui](http://schemas.microsoft.com/dotnet/2021/maui)" xmlns:x="[http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)">

    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="15">
            <Label FontAttributes="Bold" FontSize="26" HorizontalOptions="Center" Text="App Hotel - Reserva"/>

            <Label FontAttributes="Bold" Text="Escolha a Acomodação:"/>
            <Picker Title="Selecione o tipo de suíte" x:Name="pckQuarto">
                <Picker.ItemsSource>
                    <x:Array Type="{x:Type x:String}">
                        <x:String>Suíte Luxo (R$ 250/dia)</x:String>
                        <x:String>Suíte Executive (R$ 180/dia)</x:String>
                        <x:String>Suíte Crise (R$ 100/dia)</x:String>
                    </x:Array>
                </Picker.ItemsSource>
            </Picker>

            <Grid ColumnDefinitions="*, Auto" RowDefinitions="Auto, Auto" RowSpacing="10">
                <Label Grid.Column="0" Grid.Row="0" Text="{Binding Source={x:Reference stpAdultos}, Path=Value, StringFormat='Adultos: {0:F0}'}"/>
                <Stepper Grid.Column="1" Grid.Row="0" Increment="1" Maximum="10" Minimum="1" Value="1" x:Name="stpAdultos"/>

                <Label Grid.Column="0" Grid.Row="1" Text="{Binding Source={x:Reference stpCriancas}, Path=Value, StringFormat='Crianças: {0:F0}'}"/>
                <Stepper Grid.Column="1" Grid.Row="1" Increment="1" Maximum="10" Minimum="0" Value="0" x:Name="stpCriancas"/>
            </Grid>

            <Label FontAttributes="Bold" Text="Data de Check-In:"/>
            <DatePicker Format="dd/MM/yyyy" x:Name="dtpCheckIn"/>

            <Label FontAttributes="Bold" Text="Data de Check-Out:"/>
            <DatePicker Format="dd/MM/yyyy" x:Name="dtpCheckOut"/>

            <Button BackgroundColor="#2196F3" Clicked="OnCalcularClicked" Text="Avançar para Cálculo" TextColor="White"/>
            <Button BackgroundColor="#607D8B" Clicked="OnSobreClicked" Text="Sobre o Desenvolvedor" TextColor="White"/>
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>

<?xml version="1.0" encoding="utf-8" ?>
<ContentPage Title="Sobre o Desenvolvedor" x:Class="MauiAppHotel.Views.SobrePage" xmlns="[http://schemas.microsoft.com/dotnet/2021/maui](http://schemas.microsoft.com/dotnet/2021/maui)" xmlns:x="[http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)">

    <ScrollView>
        <VerticalStackLayout Padding="30" Spacing="20" VerticalOptions="Center">
            <Frame BorderColor="#2196F3" CornerRadius="15" Padding="20">
                <VerticalStackLayout HorizontalOptions="Center" Spacing="10">
                    <Label FontAttributes="Bold" FontSize="28" Text="App Hotel" TextColor="#2196F3"/>
                    <Label FontSize="14" Text="Versão 1.0 — 2026" TextColor="Gray"/>
                    
                    <Label FontAttributes="Bold" FontSize="16" Text="Desenvolvedor:"/>
                    <Label FontAttributes="Bold" FontSize="18" Text="Donizete dos Santos"/>

                    <Label FontAttributes="Bold" FontSize="16" Text="Curso:"/>
                    <Label Text="Técnico em Desenvolvimento de Sistemas (DS2)"/>
                    <Label Text="Centro Paula Souza - ETEC"/>

                    <Label FontAttributes="Bold" FontSize="16" Text="Professor Mediador:"/>
                    <Label Text="Tadeu Fernando Piran"/>
                </VerticalStackLayout>
            </Frame>
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
