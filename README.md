import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Table } from "@/components/ui/table";
import { Input } from "@/components/ui/input";
import { Select, SelectItem, SelectContent, SelectTrigger } from "@/components/ui/select";

export default function EscalaColaboradores() {
  const [colaboradores, setColaboradores] = useState([]);
  const [novoColaborador, setNovoColaborador] = useState({
    nome: "",
    dia: "",
    almocoEntrada: "",
    almocoSaida: "",
    turno: "",
    funcao: "",
    numeroCaixa: ""
  });
  const [editandoId, setEditandoId] = useState(null);

  const adicionarColaborador = () => {
    if (editandoId !== null) {
      setColaboradores(
        colaboradores.map((colaborador) =>
          colaborador.id === editandoId ? { ...novoColaborador, id: editandoId } : colaborador
        )
      );
      setEditandoId(null);
    } else {
      setColaboradores([...colaboradores, { ...novoColaborador, id: Date.now() }]);
    }
    setNovoColaborador({
      nome: "",
      dia: "",
      almocoEntrada: "",
      almocoSaida: "",
      turno: "",
      funcao: "",
      numeroCaixa: ""
    });
  };

  return (
    <div className="p-4 min-h-screen bg-gray-600 flex flex-col items-center">
      <h1 className="text-2xl font-bold mb-4 text-white">Fort Atacadista</h1>
      <h1 className="text-xl font-bold mb-4 text-white">Escala de Colaboradores - Loja 635 Jundiaí</h1>
      <Card className="w-full max-w-4xl">
        <CardContent className="grid grid-cols-3 gap-2 p-4">
          <Input placeholder="Nome" value={novoColaborador.nome} onChange={(e) => setNovoColaborador({ ...novoColaborador, nome: e.target.value })} />
          <Input placeholder="Dia" type="date" value={novoColaborador.dia} onChange={(e) => setNovoColaborador({ ...novoColaborador, dia: e.target.value })} />
          <Select value={novoColaborador.turno} onValueChange={(value) => setNovoColaborador({ ...novoColaborador, turno: value })}>
            <SelectTrigger className="w-full">{novoColaborador.turno || "Selecione um turno"}</SelectTrigger>
            <SelectContent>
              <SelectItem value="06:40 - 15:00">06:40 - 15:00</SelectItem>
              <SelectItem value="08:00 - 16:20">08:00 - 16:20</SelectItem>
              <SelectItem value="11:00 - 19:20">11:00 - 19:20</SelectItem>
              <SelectItem value="13:00 - 21:20">13:00 - 21:20</SelectItem>
              <SelectItem value="14:30 - 22:50">14:30 - 22:50</SelectItem>
            </SelectContent>
          </Select>
          <Select value={novoColaborador.funcao} onValueChange={(value) => setNovoColaborador({ ...novoColaborador, funcao: value })}>
            <SelectTrigger className="w-full">{novoColaborador.funcao || "Selecione uma função"}</SelectTrigger>
            <SelectContent>
              <SelectItem value="Operador(a)">Operador(a)</SelectItem>
              <SelectItem value="Carrinhos">Carrinhos</SelectItem>
              <SelectItem value="Self">Self</SelectItem>
              <SelectItem value="Sac">Sac</SelectItem>
              <SelectItem value="Assistentes">Assistentes</SelectItem>
            </SelectContent>
          </Select>
          {(novoColaborador.funcao === "Operador(a)" || novoColaborador.funcao === "Self") && (
            <Select value={novoColaborador.numeroCaixa} onValueChange={(value) => setNovoColaborador({ ...novoColaborador, numeroCaixa: value })}>
              <SelectTrigger className="w-full">{novoColaborador.numeroCaixa || "Selecione o número do caixa"}</SelectTrigger>
              <SelectContent>
                {[...Array(28).keys()].map(num => (
                  <SelectItem key={num + 1} value={`${num + 1}`}>{num + 1}</SelectItem>
                ))}
              </SelectContent>
            </Select>
          )}
          <Input placeholder="Entrada Almoço" type="time" value={novoColaborador.almocoEntrada} onChange={(e) => setNovoColaborador({ ...novoColaborador, almocoEntrada: e.target.value })} />
          <Input placeholder="Saída Almoço" type="time" value={novoColaborador.almocoSaida} onChange={(e) => setNovoColaborador({ ...novoColaborador, almocoSaida: e.target.value })} />
          <Button onClick={adicionarColaborador}>{editandoId ? "Salvar" : "Adicionar"}</Button>
        </CardContent>
      </Card>
    </div>
  );
}
