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
    diasTrabalho: "",
    entrada: "",
    saida: "",
    almocoEntrada: "",
    almocoSaida: "",
    caixa: "",
    folga: "",
    cargo: "",
    status: "",
    dataCriacao: new Date().toLocaleDateString("pt-BR"),
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
      setColaboradores([...colaboradores, { ...novoColaborador, id: Date.now(), dataCriacao: new Date().toLocaleDateString("pt-BR") }]);
    }
    setNovoColaborador({
      nome: "",
      diasTrabalho: "",
      entrada: "",
      saida: "",
      almocoEntrada: "",
      almocoSaida: "",
      caixa: "",
      folga: "",
      cargo: "",
      status: "",
      dataCriacao: new Date().toLocaleDateString("pt-BR"),
    });
  };

  const excluirColaborador = (id) => {
    setColaboradores(colaboradores.filter((colaborador) => colaborador.id !== id));
  };

  const editarColaborador = (colaborador) => {
    setNovoColaborador(colaborador);
    setEditandoId(colaborador.id);
  };

  const imprimirEscala = () => {
    window.print();
  };

  return (
    <div className="p-4 min-h-screen bg-gradient-to-r from-red-500 to-yellow-500 flex flex-col items-center">
      <h1 className="text-2xl font-bold mb-4 text-white">Fort Atacadista</h1>
      <h1 className="text-xl font-bold mb-4 text-white">Escala de Colaboradores - Loja 635 Jundiaí</h1>
      <Card className="w-full max-w-4xl">
        <CardContent className="grid grid-cols-3 gap-2 p-4">
          <Input
            placeholder="Nome"
            value={novoColaborador.nome}
            onChange={(e) => setNovoColaborador({ ...novoColaborador, nome: e.target.value })}
          />
          <Input
            placeholder="Dias de Trabalho"
            value={novoColaborador.diasTrabalho}
            onChange={(e) => setNovoColaborador({ ...novoColaborador, diasTrabalho: e.target.value })}
          />
          <Input
            placeholder="Entrada"
            type="time"
            value={novoColaborador.entrada}
            onChange={(e) => setNovoColaborador({ ...novoColaborador, entrada: e.target.value })}
          />
          <Input
            placeholder="Saída"
            type="time"
            value={novoColaborador.saida}
            onChange={(e) => setNovoColaborador({ ...novoColaborador, saida: e.target.value })}
          />
          <Input
            placeholder="Almoço Entrada"
            type="time"
            value={novoColaborador.almocoEntrada}
            onChange={(e) => setNovoColaborador({ ...novoColaborador, almocoEntrada: e.target.value })}
          />
          <Input
            placeholder="Almoço Saída"
            type="time"
            value={novoColaborador.almocoSaida}
            onChange={(e) => setNovoColaborador({ ...novoColaborador, almocoSaida: e.target.value })}
          />
          <Input
            placeholder="Caixa (1-28 ou Autoatendimento)"
            value={novoColaborador.caixa}
            onChange={(e) => setNovoColaborador({ ...novoColaborador, caixa: e.target.value })}
          />
          <Input
            placeholder="Cargo"
            value={novoColaborador.cargo}
            onChange={(e) => setNovoColaborador({ ...novoColaborador, cargo: e.target.value })}
          />
          <Select
            value={novoColaborador.status}
            onValueChange={(value) => setNovoColaborador({ ...novoColaborador, status: value })}
          >
            <SelectTrigger className="w-full">{novoColaborador.status || "Selecione um status"}</SelectTrigger>
            <SelectContent>
              <SelectItem value="Trabalhando">Trabalhando</SelectItem>
              <SelectItem value="Folga">Folga</SelectItem>
              <SelectItem value="Atestado">Atestado</SelectItem>
              <SelectItem value="Férias">Férias</SelectItem>
              <SelectItem value="Afastado">Afastado</SelectItem>
            </SelectContent>
          </Select>
          <Button onClick={adicionarColaborador}>{editandoId ? "Salvar" : "Adicionar"}</Button>
        </CardContent>
      </Card>
      <Table className="mt-4 w-full max-w-4xl">
        <thead>
          <tr>
            <th>Nome</th>
            <th>Dias</th>
            <th>Entrada</th>
            <th>Saída</th>
            <th>Almoço</th>
            <th>Caixa</th>
            <th>Cargo</th>
            <th>Status</th>
            <th>Data</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          {colaboradores.map((colaborador) => (
            <tr key={colaborador.id}>
              <td>{colaborador.nome}</td>
              <td>{colaborador.diasTrabalho}</td>
              <td>{colaborador.entrada}</td>
              <td>{colaborador.saida}</td>
              <td>{colaborador.almocoEntrada} - {colaborador.almocoSaida}</td>
              <td>{colaborador.caixa}</td>
              <td>{colaborador.cargo}</td>
              <td>{colaborador.status}</td>
              <td>{colaborador.dataCriacao}</td>
              <td>
                <Button onClick={() => editarColaborador(colaborador)}>Editar</Button>
                <Button onClick={() => excluirColaborador(colaborador.id)} variant="destructive">Excluir</Button>
              </td>
            </tr>
          ))}
        </tbody>
      </Table>
      <Button className="mt-4" onClick={imprimirEscala}>Imprimir</Button>
    </div>
  );
}
